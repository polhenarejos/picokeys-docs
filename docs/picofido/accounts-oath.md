# Accounts (OATH)

This page describes the OATH account model in Pico FIDO (TOTP/HOTP).

---

## Overview

Pico FIDO can store OATH accounts and generate one-time codes for:

- TOTP (time-based OTP)
- HOTP (counter-based OTP)

Accounts are typically identified by issuer + account label and are managed on-device.

---

## TOTP accounts

TOTP codes are derived from:

- Shared secret
- Time step (period, usually 30 seconds)
- Output digit length (typically 6 or 8 digits)

Use TOTP when both authenticator and verifier share clock-based code rotation.

---

## HOTP accounts

HOTP codes are derived from:

- Shared secret
- Moving counter
- Output digit length (typically 6 or 8 digits)

Use HOTP when the verifier expects counter-synchronized OTP values.

!!! warning
    HOTP verification depends on counter synchronization between device and server.

---

## Account lifecycle

Typical lifecycle operations are:

- Create account
- List account metadata
- Generate/emit OTP
- Delete account

When account storage is locked by policy, unlock/verification may be required before listing or using accounts.

---

## Security considerations

- Secrets are stored on-device.
- Protect account management with PIN/policy where available.
- Prefer TOTP/HOTP only for services that still require OTP workflows; use passkeys where possible for phishing-resistant auth.
