---
type: TC
id: TC-002
title: Urgent-low threshold breach produces URGENT alarm
slug: tc-urgent-low-emits
relations:
  - type: TESTS
    target: SRS-002
---

# TC-002 — Urgent-low threshold breach produces URGENT alarm

## Preconditions
- Alarm engine is reset for tests.
- Settings: `alarmUrgentLow = true`, `alarmLow = true`.
- Thresholds: `bgLow = 55` mg/dL, `bgTargetBottom = 80` mg/dL.
- Latest CGM reading: `{ mgdl: 50, mills: T }` where T is current sandbox time.

## Steps
1. Construct a sandbox carrying the preconditions.
2. Invoke `simplealarms.checkNotifications(sbx)`.

## Expected result
- A single notification is requested via `sbx.notifications.requestNotify`.
- The notification has `level == levels.URGENT`, `eventName == "low"`, and `pushoverSound == "persistent"`.
- The title resolves through `ctx.levels.toDisplay(URGENT) + " LOW"`.
- No notification is requested when SGV is set to 55 mg/dL exactly (strict less-than boundary).
