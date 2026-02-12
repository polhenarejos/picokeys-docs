# Pico FIDO

Pico FIDO transforms supported hardware (such as Raspberry Pi Pico or ESP32) into a USB authenticator compatible with modern passkey workflows.

---

## Purpose and scope

Pico FIDO is designed for:

- WebAuthn / FIDO2 authentication
- CTAP2 and legacy U2F compatibility
- Discoverable credential management
- OTP and OATH workflows in compatible configurations

!!! note
    Pico FIDO is an authenticator firmware, not a general-purpose cryptographic API.

---

## Supported standards and features

Pico FIDO includes support for:

- CTAP 2.1 and CTAP 1
- WebAuthn and U2F flows
- PIN-based user verification
- User presence checks (physical confirmation)
- Resident/discoverable credentials
- Credential management operations
- Extensions such as `hmac-secret`, `credProtect`, `minPinLength`, `credBlob`, and large blob support

---

## Cryptography and curves

Authentication keys support ECDSA (and EdDSA when enabled by firmware/build), including commonly used curves such as:

- `secp256r1`
- `secp384r1`
- `secp521r1`
- `secp256k1`
- `Ed25519`

---

## Security model highlights

- Private keys stay on-device
- PIN policy and retries are enforced by the authenticator
- Optional secure lock / configuration controls are available depending on firmware

!!! warning
    Some security options can be difficult or impossible to undo without reset and full reprovisioning.

---

## Detailed guides

For practical usage, see:

- [Initialization and policy](initialization.md)
- [Passkeys](passkeys.md)
- [Accounts (OATH)](accounts-oath.md)
- [Slots & OTP](slots-otp.md)
