# OATH accounts

Pico FIDO also implements OATH-style one-time password storage. This is useful, but it is not why most people should choose the firmware.

## What upstream claims

The upstream README lists support for:

- OATH based on the YKOATH protocol
- TOTP
- HOTP
- compatibility with Yubico Authenticator and `ykman` in supported configurations

This is a practical feature set for users who still need OTP-based services.

## When OATH makes sense

Use OATH accounts when:

- the service does not support passkeys
- you need TOTP or HOTP specifically
- you want the secrets to live on the device instead of in a phone authenticator app

Do not confuse that with phishing resistance. OATH codes remain weaker than passkeys because the user can still be tricked into entering a valid code on the wrong site.

## TOTP vs HOTP

### TOTP

Time-based OTP is the common case:

- rotating code derived from shared secret and current time step
- typically 6 or 8 digits
- simple for users, but depends on clock agreement

### HOTP

Counter-based OTP is narrower:

- rotating code derived from shared secret and moving counter
- works without time synchronization
- more operationally fragile if client and verifier counters drift

If you do not explicitly need HOTP, TOTP is usually easier to live with.

## Operational cautions

- account storage is finite
- account metadata may be managed differently depending on the tool
- Yubico-oriented apps may care about VID/PID identity or device naming heuristics

That means a feature can exist in firmware and still be awkward in a specific desktop toolchain.

## Security notes

- the secret stays on the device
- the generated code does not
- whoever sees a valid OTP quickly enough can usually replay it

Treat OATH as a compatibility feature, not as the primary reason to adopt Pico FIDO.
