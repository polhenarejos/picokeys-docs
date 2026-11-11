# OATH accounts

Pico FIDO also stores OATH accounts for services that still require one-time passwords. Most users choose the firmware for its FIDO features, but OATH can help with older services.

## What upstream claims

The upstream README lists support for:

- OATH based on the YKOATH protocol
- TOTP
- HOTP
- compatibility with Yubico Authenticator and `ykman` in supported configurations

Together, these features cover users who still need OTP-based services.

## When OATH makes sense

OATH accounts are useful when:

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

As a result, a feature may work in firmware but remain awkward to manage with a particular desktop tool.

## Security notes

- the secret stays on the device
- the generated code does not
- anyone who sees a valid OTP may be able to use it before the legitimate user does

Treat OATH as a compatibility feature, not as the primary reason to adopt Pico FIDO.
