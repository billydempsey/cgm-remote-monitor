---
type: TC
id: TC-003
title: Stale CGM data does not trigger threshold alarm
slug: tc-stale-data-no-alarm
relations:
  - type: TESTS
    target: SRS-003
---

# TC-003 — Stale CGM data does not trigger threshold alarm

## Preconditions
- Alarm engine is reset for tests.
- All four BG alarm enable settings are true.
- Thresholds set so that an SGV of 280 mg/dL would trigger an URGENT high alarm.
- Sandbox time T = current.
- Latest CGM reading: `{ mgdl: 280, mills: T - 600001 }` (10 minutes and 1 millisecond old).

## Steps
1. Construct a sandbox carrying the preconditions.
2. Invoke `simplealarms.checkNotifications(sbx)`.

## Expected result
- `sbx.notifications.requestNotify` is not invoked.
- No notification record is produced.
- A subsequent invocation with the reading timestamp updated to `T - 599999` (under 10 minutes old) does invoke `requestNotify`, confirming that the gate is purely freshness-based.
