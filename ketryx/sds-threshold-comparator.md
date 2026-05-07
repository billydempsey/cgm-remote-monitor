---
type: SDS
id: SDS-001
title: Threshold comparator and freshness gate
slug: sds-threshold-comparator
relations:
  - type: FULFILLS
    target: SRS-001
  - type: FULFILLS
    target: SRS-002
  - type: FULFILLS
    target: SRS-003
---

# SDS-001 — Threshold comparator and freshness gate

The threshold comparator (`lib/plugins/simplealarms.js` `checkNotifications` and `compareBGToTresholds`) reads the most recent CGM reading from the sandbox (`sbx.lastSGVEntry()`), scales the value to the user's display units (`sbx.scaleEntry`), and compares the scaled value against four user-configured thresholds: urgent-high (`thresholds.bgHigh`), warning-high (`thresholds.bgTargetTop`), warning-low (`thresholds.bgTargetBottom`), and urgent-low (`thresholds.bgLow`).

A freshness gate precedes comparison: the comparator abandons evaluation when `sbx.time - lastSGVEntry.mills` is greater than or equal to 10 minutes (`times.mins(10).msecs`). It additionally rejects sentinel error values by requiring `lastSGVEntry.mgdl > 39` before comparison.

When a threshold is breached and the corresponding enable flag (`alarmUrgentHigh`, `alarmHigh`, `alarmLow`, `alarmUrgentLow`) is set, the comparator constructs a notification record carrying the alarm level (URGENT for urgent thresholds, WARN for non-urgent), the direction (`eventName` of `"high"` or `"low"`), the originating plugin reference, a Pushover priority sound hint (`persistent` for URGENT, `climb` or `falling` for directional WARN), and a debug payload containing the scaled SGV and the threshold values. The record is passed to `sbx.notifications.requestNotify` for downstream dispatch.
