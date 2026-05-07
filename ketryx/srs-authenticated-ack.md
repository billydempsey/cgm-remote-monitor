---
type: SRS
id: SRS-005
title: System shall require authentication before accepting global alarm acknowledgements
slug: srs-authenticated-ack
relations:
  - type: HAS_PARENT
    target: SUB-003
---

# SRS-005 — Authentication required for global acknowledgement

The system shall reject any alarm acknowledgement message received from a client that has not presented valid authentication credentials granting the notifications acknowledgement permission.

**Pass criterion:** Given a client that has not presented valid authentication credentials, when the client sends an alarm acknowledgement message, the system shall not record the acknowledgement, shall not propagate a clear notification to other subscribed clients, and shall not extend the alarm silence interval.
