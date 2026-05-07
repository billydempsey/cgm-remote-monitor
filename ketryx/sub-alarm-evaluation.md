---
type: SUB
id: SUB-001
title: Alarm Evaluation Subsystem
slug: sub-alarm-evaluation
relations:
  - type: HAS_PARENT
    target: SYS-001
---

# SUB-001 — Alarm Evaluation Subsystem

The Alarm Evaluation Subsystem shall evaluate each new CGM reading against configured BG thresholds and forecast risk indicators and shall produce an alarm level and direction whenever the reading or forecast violates the configured limits. The subsystem shall reject readings that are older than 10 minutes relative to the evaluation time and shall reject sensor-error sentinel values.
