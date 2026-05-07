---
type: SDS
id: SDS-002
title: Socket.IO alarm namespace broadcaster
slug: sds-alarm-broadcaster
relations:
  - type: FULFILLS
    target: SRS-004
---

# SDS-002 — Socket.IO alarm namespace broadcaster

The alarm broadcaster (`lib/api3/alarmSocket.js` `AlarmSocket`) registers a listener on the internal event bus (`ctx.bus.on('notification', self.emitNotification)`) at namespace initialization time. On each `notification` event, `emitNotification` inspects the notification record and dispatches an event on the `/alarm` Socket.IO namespace as follows:

- `clear: true` is emitted as `clear_alarm`.
- Level equal to `levels.WARN` is emitted as `alarm`.
- Level equal to `levels.URGENT` is emitted as `urgent_alarm`.
- `isAnnouncement: true` is emitted as `announcement`.
- All other records are emitted as `notification`.

Each emission is a broadcast (`self.namespace.emit`) reaching every socket currently subscribed to the namespace. Authentication of subscribers occurs at subscribe time (see SDS-003); broadcast does not perform per-recipient authorization. Client-side handlers in `lib/client/index.js` (`alarmSocket.on('alarm')`, `alarmSocket.on('urgent_alarm')`, `alarmSocket.on('clear_alarm')`) translate the socket events into local audio playback and visual state changes.

Latency from event-bus emission to socket emission is bounded by Socket.IO's synchronous emit path; under nominal load the round-trip from server bus emission to client receipt is well under 2 seconds.
