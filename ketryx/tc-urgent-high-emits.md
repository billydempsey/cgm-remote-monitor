---
type: TC
id: TC-001
title: Urgent-high threshold breach produces URGENT alarm
slug: tc-urgent-high-emits
relations:
  - type: TESTS
    target: SRS-001
---

# TC-001 — Urgent-high threshold breach produces URGENT alarm

## Preconditions
- Alarm engine is reset for tests (`notifications.resetStateForTests()` available).
- Settings: `alarmUrgentHigh = true`, `alarmHigh = true`.
- Thresholds: `bgHigh = 260` mg/dL, `bgTargetTop = 180` mg/dL.
- Sandbox time T = arbitrary recent time.
- Latest CGM reading: `{ mgdl: 280, mills: T }`.

## Steps
1. Construct a sandbox carrying the preconditions.
2. Invoke `simplealarms.checkNotifications(sbx)`.

## Expected result
- A single notification is requested via `sbx.notifications.requestNotify`.
- The notification has `level == levels.URGENT`, `eventName == "high"`, and `pushoverSound == "persistent"`.
- The title resolves through `ctx.levels.toDisplay(URGENT) + " HIGH"`.
- No notification is requested when SGV is set to 260 mg/dL exactly (boundary check; comparator uses strict greater-than).
