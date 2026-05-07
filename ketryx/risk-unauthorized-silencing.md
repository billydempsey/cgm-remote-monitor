---
type: Risk
id: RISK-003
title: Unauthorized actor silences active alarm globally
slug: risk-unauthorized-silencing
hazard_type: Operational hazard — unauthorized alarm suppression
relations:
  - type: IS_RISK_CONTROLLED_BY
    target: SRS-005
---

# RISK-003 — Unauthorized actor silences active alarm globally

## Hazard / Hazard Type
Operational and information-security hazard — an unauthorized client suppresses an active alarm for all subscribed clients. Default ISO 14971 hazard taxonomy applied.

## Hazardous Situation
Patient is in or about to enter a dangerous glucose state while the global alarm has been cleared by an unauthorized actor, so legitimate clients no longer present the alert.

## Sequence of Events
1. An unauthenticated actor establishes a connection to the alarm broadcast channel.
2. The actor sends an acknowledgement message naming the active alarm and a snooze duration.
3. If the system did not enforce authentication on acknowledgement, the system would record the acknowledgement, suppress further emissions, and broadcast a clear notification to all other subscribed clients.
4. Legitimate clients silence their local alarm in response to the clear notification.

## Harm
Missed urgent alert with downstream physical harm scenarios as described in RISK-001 and RISK-002.

## Risk Control
Authenticated acknowledgement gate.

## Risk Controls Description
SRS-005 requires valid authentication credentials and the notifications acknowledgement permission before an acknowledgement is accepted. Acknowledgement listeners are bound to a socket only after the authentication and permission checks succeed; messages from unauthenticated sockets are discarded before reaching the central acknowledgement function, so no silence interval is recorded and no clear notification is broadcast.

## Residual risk
Low — control fully prevents the unauthenticated path on the authenticated web-client flow when the authentication-prompt-on-load setting is enabled. Residual concerns relate to credential handling, which is out of scope.
