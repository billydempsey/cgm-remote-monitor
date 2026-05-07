---
type: SUB
id: SUB-002
title: Alarm Dispatch Subsystem
slug: sub-alarm-dispatch
relations:
  - type: HAS_PARENT
    target: SYS-002
---

# SUB-002 — Alarm Dispatch Subsystem

The Alarm Dispatch Subsystem shall publish every alarm produced by the Alarm Evaluation Subsystem to every client subscribed to the dedicated alarm broadcast channel and shall route alarms with severity warning or urgent through external high-priority push channels when those channels are configured.
