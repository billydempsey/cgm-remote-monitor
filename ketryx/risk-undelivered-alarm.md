---
type: Risk
id: RISK-002
title: Issued alarm not received by any active client
slug: risk-undelivered-alarm
hazard_type: Software hazard — alarm not delivered to user
relations:
  - type: IS_RISK_CONTROLLED_BY
    target: SRS-004
---

# RISK-002 — Issued alarm not received by any active client

## Hazard / Hazard Type
Software hazard — alarm produced by the server is not received by any user-facing client. Default ISO 14971 hazard taxonomy applied.

## Hazardous Situation
Patient is in a hyperglycemic or hypoglycemic state while the alarm intended to notify the patient or caregiver is not received by any active client device.

## Sequence of Events
1. Server detects an out-of-range glucose condition and issues an alarm.
2. The user's client device has lost connectivity (network drop, application backgrounded with the alarm channel closed, or device asleep without push).
3. The alarm broadcast on the dedicated channel is not received by the client.
4. No audible or visible alert is produced on that device, and the user does not learn of the condition.

## Harm
Delayed therapy adjustment, leading to prolonged hyper- or hypoglycemia and the same downstream physical harms described in RISK-001.

## Risk Control
Multi-path alarm dispatch — broadcast over the dedicated alarm channel to all subscribed clients, complemented by external high-priority push when configured.

## Risk Controls Description
SRS-004 requires that every issued alarm be transmitted to every client currently subscribed to the alarm broadcast channel within 2 seconds of issuance. In addition, when a high-priority external push channel is configured, alarms of severity warning or urgent are dispatched through that channel using emergency priority with retry, providing a delivery path that does not depend on an open in-application socket.

## Control gap
A user with no clients connected and no external push configured has no delivery path. The broadcast channel does not provide store-and-forward; if no client is connected at the moment of emission, the alarm is not later replayed on reconnection.

## Residual risk
Medium — multi-channel dispatch reduces but does not eliminate the chance of non-delivery.
