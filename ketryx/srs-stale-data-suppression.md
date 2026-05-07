---
type: SRS
id: SRS-003
title: System shall suppress threshold alarms on stale CGM data
slug: srs-stale-data-suppression
relations:
  - type: HAS_PARENT
    target: SUB-001
---

# SRS-003 — Suppression of threshold alarms on stale data

The system shall not raise a threshold-based blood glucose alarm when the timestamp of the most recent CGM reading is more than 10 minutes earlier than the current evaluation time.

**Pass criterion:** Given a CGM reading whose timestamp is at least 10 minutes and 1 millisecond earlier than the evaluation time, when the alarm evaluator runs against any threshold configuration that would otherwise produce an alarm, no alarm notification record shall be produced.
