---
type: Risk
id: RISK-004
title: Alarm fatigue leads user to disable alerts
slug: risk-realarm-fatigue
hazard_type: Use-related hazard — alarm fatigue
relations:
  - type: IS_RISK_CONTROLLED_BY
    target: SRS-006
---

# RISK-004 — Alarm fatigue leads user to disable alerts

## Hazard / Hazard Type
Use-related hazard — repeated identical alarms during a single sustained excursion lead the user to disable alarms entirely, so subsequent distinct excursions are not announced. Default ISO 14971 hazard taxonomy applied.

## Hazardous Situation
A user has disabled the alarm system in response to perceived noise during a prior sustained glucose excursion and is now in or entering a new dangerous glucose excursion that will not produce any audible or visible alert.

## Sequence of Events
1. Patient enters a sustained hyperglycemic or hypoglycemic state lasting tens of minutes.
2. Each evaluation cycle re-evaluates the same condition.
3. Without re-emission suppression, each cycle would re-emit the same alarm to all subscribed clients.
4. The user, overwhelmed by the repeated audible and visible alerts, disables the alarm setting or mutes the device.
5. A later, distinct dangerous excursion occurs and is not announced because alarms remain disabled or muted.

## Harm
Missed alarm for the later excursion, leading to the same downstream physical harms described in RISK-001.

## Risk Control
Snooze and silence interval enforcement.

## Risk Controls Description
SRS-006 suppresses re-emission of alarms whose severity and group match an acknowledged alarm for the duration specified by the acknowledging user (with a default of 30 minutes when none is specified). This prevents repeated alerts for the same ongoing condition while permitting new conditions of higher severity or different group to surface.

## Residual risk
Medium — snooze suppression mitigates within-condition repetition but does not address fatigue from many distinct alarms over a longer period.
