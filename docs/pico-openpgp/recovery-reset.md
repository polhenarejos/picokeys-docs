# Recovery and reset

Recovery is where the role split between `PW1`, `PW3`, and the reset code stops being theory.

## User-PIN recovery

If `PW1` becomes blocked, the intended recovery path is through the administrative or reset-code mechanism, depending on how the card was provisioned.

That recovery path should be tested before the card holds important material.

## Admin lockout

An admin-path problem is more serious than a user-path problem because it blocks key and policy management, not just day-to-day use.

Do not assume you can improvise recovery later. Write the runbook down while the setup is still fresh.

## Factory reset

A full reset is the cleanest way to prove what the applet really wipes:

- keys
- PIN state
- cardholder metadata
- recovery material

Run this test on non-critical hardware at least once. Otherwise the reset story is still theoretical.

## Conservative rule

If you have not tested both a blocked-user recovery and one full reset path, your operational documentation is incomplete.
