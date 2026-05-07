---
type: Risk
id: RISK-001
title: Missed hypoglycemic alert due to stale CGM data treated as current
slug: risk-stale-data-missed-hypo
hazard_type: Information hazard — missed alert due to stale source data
relations:
  - type: IS_RISK_CONTROLLED_BY
    target: SRS-003
---

# RISK-001 — Missed hypoglycemic alert due to stale CGM data

## Hazard / Hazard Type
Information hazard — the absence of fresh CGM data could lead the system to either omit a needed alert or, in the inverse case, raise a stale-but-out-of-range value as a fresh alert. Default ISO 14971 hazard taxonomy applied; no project risk management policy was located in the repository.

## Hazardous Situation
Patient is hypoglycemic while the monitoring application has stopped receiving recent CGM readings, so threshold-based alerting is not driven by current physiology.

## Sequence of Events
1. CGM data delivery to the server is interrupted (sensor signal loss, transmitter disconnect, uploader application terminated, or network outage).
2. The most recent stored reading continues to age while patient glucose evolves independently.
3. Patient glucose drops into hypoglycemia.
4. Because there is no fresh reading, threshold-based evaluation cannot detect the hypoglycemia and no alarm is raised.

## Harm
Untreated severe hypoglycemia, with risk of seizure, cognitive impairment, loss of consciousness, or death.

## Risk Control
Stale-data alarm suppression and separate Time Ago alarm channel.

## Risk Controls Description
SRS-003 requires that threshold-based alarms not be produced when the most recent reading is more than 10 minutes old, preventing false reassurance from a stale value misinterpreted as fresh. A separate Time Ago alarm (raised on the client when no fresh reading has been received for the configured interval) notifies the user that the data feed has stopped, prompting fingerstick or alternate measurement.

## Control gap
SRS-003 alone does not raise a stale-data alarm; it only suppresses threshold-based ones. The Time Ago alarm is implemented client-side; if every client is closed, muted, or offline, no alert reaches the user. Recommend a server-side stale-data alert raised through SUB-002 dispatch and external push channels.

## Residual risk
Medium — control reduces the chance of false reassurance but does not guarantee notification of the data outage to the user.
