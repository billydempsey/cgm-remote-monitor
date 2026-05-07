---
type: SRS
id: SRS-002
title: System shall raise urgent alarm on urgent-low BG threshold breach
slug: srs-urgent-low-bg-alarm
relations:
  - type: HAS_PARENT
    target: SUB-001
---

# SRS-002 — Urgent alarm on urgent-low threshold breach

The system shall raise an urgent-priority blood glucose alarm when the most recent CGM reading is below the user-configured urgent-low glucose threshold and the user-configured urgent-low alarm setting is enabled.

**Pass criterion:** Given the user-configured urgent-low threshold Y mg/dL and an urgent-low enable flag set to true, when a CGM reading with a value strictly less than Y is presented to the alarm evaluator and the reading is less than 10 minutes old, the system shall produce exactly one notification record with severity equal to "urgent" and direction equal to "low".
