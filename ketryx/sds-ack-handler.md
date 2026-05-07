---
type: SDS
id: SDS-003
title: Authenticated acknowledgement handler and silence-time tracker
slug: sds-ack-handler
relations:
  - type: FULFILLS
    target: SRS-005
  - type: FULFILLS
    target: SRS-006
---

# SDS-003 — Authenticated acknowledgement handler and silence-time tracker

Acknowledgement handling spans the alarm namespace handler (`lib/api3/alarmSocket.js` `subscribe`) and the central notifications module (`lib/notifications.js`).

## Authentication gate
Subscription to the `/alarm` namespace authenticates by one of two paths:

- **Native client path:** message contains `accessToken`; the system calls `ctx.authorization.resolveAccessToken(message.accessToken, …)`. On success, an `ack` listener is bound on the socket that forwards to `ctx.notifications.ack(level, group, silenceTime, true)`.
- **Web client path:** message contains `secret` or `jwtToken`; the system calls `ctx.authorization.resolve(…)`. On success, the system computes a permission set including `ack: ctx.authorization.checkMultiple('notifications:*:ack', auth.shiros)`. The `ack` listener is bound; the listener forwards to the central acknowledgement function only when `perms.ack` is true. Without the permission, the `ack` event is dropped.

When `env.settings.authenticationPromptOnLoad` is true, web-client connections without `secret` or `jwtToken` are rejected before any `ack` listener is bound. Connections that fail authentication receive a callback response with `success: false` and the listener is never attached, so the central acknowledgement function is unreachable from that socket.

## Silence-time tracker
`notifications.ack(level, group, time, sendClear)` looks up or creates an `Alarm` record keyed by `level + '-' + group`. If the alarm has already been snoozed within its current `silenceTime` window the call is rejected with a console warning. Otherwise the function records `lastAckTime = Date.now()`, sets `silenceTime` to the supplied `time` value or the default `THIRTY_MINUTES` (30 × 60 × 1000 ms) when `time` is falsy, and clears `lastEmitTime` so the alarm record is no longer considered actively emitted.

When the acknowledged level is URGENT (level 2), the function recursively acknowledges level 1 (WARN) for the same group so a snooze on the urgent alarm cascades to the warning alarm.

When `sendClear` is true (always true for client-initiated acks) a synthetic `clear` notification is emitted to the bus so SDS-002 broadcasts a `clear_alarm` event to all subscribed clients.

## Re-emission suppression
`emitNotification` (in `lib/notifications.js`) gates each emission on `ctx.ddata.lastUpdated > alarm.lastAckTime + alarm.silenceTime`. While the snooze window is open, calls to emit are dropped with a console log of the remaining minutes. Snooze records (`requests.snoozes`) provide an additional gate at process time: an alarm whose level is at or below an active snooze of the same group is replaced by an automatic ack of the snoozing level, ensuring the recorded silence interval covers it.
