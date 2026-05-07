---
type: SRS
id: SRS-006
title: System shall suppress re-emission of an acknowledged alarm during the snooze interval
slug: srs-snooze-suppression
relations:
  - type: HAS_PARENT
    target: SUB-003
---

# SRS-006 — Re-emission suppression during snooze interval

Following acceptance of a valid alarm acknowledgement, the system shall suppress re-emission of any alarm whose severity and group match the acknowledged alarm for the duration specified in the acknowledgement, applying a default duration of 30 minutes when no duration is supplied.

**Pass criterion:** Given an alarm at severity S and group G that has been acknowledged with snooze duration D, when the underlying alarm condition persists or re-occurs at any time T satisfying T < ack-time + D, the system shall not emit a new alarm notification for severity S and group G; at any time T satisfying T ≥ ack-time + D, the system shall be eligible to re-emit. When D is not specified at acknowledgement time, D shall equal 30 minutes.
