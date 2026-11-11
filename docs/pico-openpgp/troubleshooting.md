# Troubleshooting

## `gpg --card-status` fails

First check whether the host can see the reader and card:

- is PC/SC running?
- is the reader visible?
- does another smart-card tool see the device?

If these checks fail, the issue is usually in the middleware or reader setup.

## The card is visible but an admin action fails

Before investigating the firmware, confirm that:

- are you authenticated as `PW3`?
- did the client keep or lose the session?
- is the operation one the client actually exposes cleanly?

## Large RSA operations seem stuck

Long delays can be normal: upstream RSA timings range from seconds to minutes, depending on the key size and operation.

If you need fast provisioning, prefer ECC unless a specific RSA requirement exists.

## A feature exists in the README but not in the client

Firmware, middleware, and client support are separate checks. Confirm each one:

1. firmware support
2. middleware support
3. client support

The feature is available in practice only when all three layers support it.

## Reset or unblock behavior is unclear

Test the recovery flow on a non-critical card and record the steps before an incident makes them urgent.
