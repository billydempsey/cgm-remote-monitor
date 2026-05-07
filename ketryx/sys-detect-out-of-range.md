---
type: SYS
id: SYS-001
title: System shall detect out-of-range glucose conditions
slug: sys-detect-out-of-range
relations:
  - type: HAS_PARENT
    target: PN-001
---

# SYS-001 — Detect out-of-range glucose conditions

The system shall detect each out-of-range or impending out-of-range glucose condition derived from the most recent CGM reading and from configured user thresholds, and shall produce an alarm record carrying severity (urgent or warning) and direction (high or low).
