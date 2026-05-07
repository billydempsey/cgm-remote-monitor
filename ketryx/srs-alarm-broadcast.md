---
type: SRS
id: SRS-004
title: System shall broadcast each issued alarm to all subscribed clients
slug: srs-alarm-broadcast
relations:
  - type: HAS_PARENT
    target: SUB-002
---

# SRS-004 — Broadcast of issued alarms to subscribed clients

The system shall transmit each issued alarm to every client subscribed to the alarm broadcast channel within 2 seconds of alarm issuance.

**Pass criterion:** Given N clients subscribed to the alarm broadcast channel, when the system issues an alarm of severity warning or urgent, all N clients shall receive a corresponding alarm event carrying the same severity, direction, title, and message within 2 seconds of issuance.
