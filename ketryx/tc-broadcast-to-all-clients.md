---
type: TC
id: TC-004
title: Issued alarm is broadcast to all subscribed Socket.IO clients
slug: tc-broadcast-to-all-clients
relations:
  - type: TESTS
    target: SRS-004
---

# TC-004 — Broadcast of issued alarm to all subscribed clients

## Preconditions
- A running server with `AlarmSocket` initialized and bound to `ctx.bus`.
- Two Socket.IO clients have connected to the `/alarm` namespace and have successfully subscribed with valid credentials. Both clients have registered listeners for `alarm`, `urgent_alarm`, and `clear_alarm`.

## Steps
1. Emit a notification record on the internal bus with `level == levels.URGENT`, group `"default"`, and a known title and message: `ctx.bus.emit('notification', notify)`.
2. Record the wall-clock timestamp at which the bus emission occurs.
3. Wait for both clients to receive a socket event or until 5 seconds have elapsed.

## Expected result
- Each of the two clients receives exactly one `urgent_alarm` event whose payload contains the same `level`, `group`, `title`, and `message` as the emitted notification.
- The receipt timestamp on each client is within 2 seconds of the bus emission timestamp.
- A subsequent test that emits a `level == levels.WARN` notification produces an `alarm` event on both clients (not `urgent_alarm`).
- A subsequent test that emits a `clear: true` notification produces a `clear_alarm` event on both clients.
