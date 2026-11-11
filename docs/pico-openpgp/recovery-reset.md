# Recovery and reset

The role split between `PW1`, `PW3`, and the reset code determines which recovery options remain available after a lockout.

## User-PIN recovery

If `PW1` becomes blocked, the intended recovery path is through the administrative or reset-code mechanism, depending on how the card was provisioned.

Test the recovery path before storing important material on the card.

## Admin lockout

An admin-path problem blocks key and policy management as well as day-to-day use, so it is more serious than a user-path problem.

Document the admin recovery steps while the setup is fresh, so operators can follow them during a lockout.

## Factory reset

Test a full reset on non-critical material to see which of these items it clears:

- keys
- PIN state
- cardholder metadata
- recovery material

Run this test on non-critical hardware at least once, then record the actual reset behavior.

## Conservative rule

Your recovery documentation is complete only after you have tested both a blocked-user recovery and a full reset path.
