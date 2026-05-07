---
type: SRS
id: SRS-001
title: System shall raise urgent alarm on urgent-high BG threshold breach
slug: srs-urgent-high-bg-alarm
relations:
  - type: HAS_PARENT
    target: SUB-001
---

# SRS-001 — Urgent alarm on urgent-high threshold breach

The system shall raise an urgent-priority blood glucose alarm when the most recent CGM reading exceeds the user-configured urgent-high glucose threshold and the user-configured urgent-high alarm setting is enabled.

**Pass criterion:** Given the user-configured urgent-high threshold X mg/dL and an urgent-high enable flag set to true, when a CGM reading with a value strictly greater than X is presented to the alarm evaluator and the reading is less than 10 minutes old, the system shall produce exactly one notification record with severity equal to "urgent" and direction equal to "high".
