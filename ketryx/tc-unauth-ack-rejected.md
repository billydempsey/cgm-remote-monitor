---
type: TC
id: TC-005
title: Acknowledgement from unauthenticated client is rejected
slug: tc-unauth-ack-rejected
relations:
  - type: TESTS
    target: SRS-005
---

# TC-005 — Acknowledgement from unauthenticated client is rejected

## Preconditions
- Server running with `env.settings.authenticationPromptOnLoad = true`.
- Authorized client A has subscribed to `/alarm` and an URGENT alarm is currently active for group `"default"`. Client A's audio is playing.
- Unauthenticated client U connects to `/alarm` and sends a subscribe message with no `secret`, no `jwtToken`, and no `accessToken`.

## Steps
1. Capture the subscribe callback response received by U.
2. Emit an `ack` event from U with `level = 2`, `group = "default"`, and `silenceTime = 1800000` (30 minutes).
3. Wait 2 seconds.
4. Inspect the alarm record state via `notifications.getAlarmForTests(2, 'default')`.
5. Inspect whether client A received any `clear_alarm` event after step 2.

## Expected result
- Step 1 callback response carries `success: false` and a missing-or-bad-token message.
- Step 2 produces no change in `alarm.lastAckTime` or `alarm.silenceTime` recorded server-side.
- Client A receives no `clear_alarm` event in step 5; its alarm remains active.
- A subsequent legitimate ack from client A successfully clears the alarm, demonstrating that the ack pathway is intact.
