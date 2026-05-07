---
type: SUB
id: SUB-003
title: Alarm Acknowledgement Subsystem
slug: sub-alarm-acknowledgement
relations:
  - type: HAS_PARENT
    target: SYS-003
---

# SUB-003 — Alarm Acknowledgement Subsystem

The Alarm Acknowledgement Subsystem shall accept acknowledgements only from authenticated users who hold the notifications acknowledgement permission, and shall track per-level, per-group silence intervals so that subsequent emissions of the acknowledged alarm are suppressed for the duration specified by the acknowledging user.
