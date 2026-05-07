---
type: TC
id: TC-006
title: Acknowledged alarm is not re-emitted during the snooze interval
slug: tc-snooze-suppresses-realarm
relations:
  - type: TESTS
    target: SRS-006
---

# TC-006 — Acknowledged alarm is not re-emitted during snooze interval

## Preconditions
- Alarm engine is reset for tests.
- An URGENT alarm at level 2, group `"default"` is active and has produced an emission to the bus.
- An authorized web client has the `notifications:*:ack` permission.

## Steps
1. The authorized client sends `ack(2, 'default', 900000)` (15-minute snooze).
2. Within 1 minute, the underlying alarm condition is re-evaluated and `simplealarms.checkNotifications` again calls `requestNotify` for the same level and group.
3. Invoke `notifications.process()`.
4. Capture all `notification` events emitted on `ctx.bus` during step 3.
5. Advance simulated time (or `ctx.ddata.lastUpdated`) to ack-time + 15 minutes + 1 second.
6. Repeat the alarm condition and invoke `notifications.process()` again.

## Expected result
- After step 1, `notifications.getAlarmForTests(2, 'default').silenceTime == 900000` and `lastAckTime` is set; one `clear_alarm` notification is emitted to the bus immediately following the ack (because `sendClear` is true).
- During step 4, no `notification` event is emitted to the bus for level 2, group `"default"`. A console log indicates the remaining snooze minutes.
- After step 6, a fresh `notification` event is emitted to the bus, confirming the alarm becomes eligible for re-emission once the snooze interval has elapsed.
- When step 1 is repeated with `silenceTime` omitted, `silenceTime` defaults to 1800000 (30 minutes).
