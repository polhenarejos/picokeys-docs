# Troubleshooting

## `gpg --card-status` fails

Start with the host stack, not the card logic:

- is PC/SC running?
- is the reader visible?
- does another smart-card tool see the device?

Most failures here are middleware problems first.

## The card is visible but an admin action fails

Check the role context before blaming the firmware:

- are you authenticated as `PW3`?
- did the client keep or lose the session?
- is the operation one the client actually exposes cleanly?

## Large RSA operations seem stuck

That can be normal. Upstream timing for RSA generation and use is measured in seconds or minutes, not milliseconds.

If you need fast provisioning, prefer ECC unless a specific RSA requirement exists.

## A feature exists in the README but not in the client

That is common in OpenPGP land. Confirm separately:

1. firmware support
2. middleware support
3. client support

Only the combination matters operationally.

## Reset or unblock behavior is unclear

If the recovery path is unclear during an incident, the documentation is already too late. Test the recovery flows on a non-critical card and document the exact steps you will actually use.
