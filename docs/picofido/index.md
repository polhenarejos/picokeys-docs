# Pico FIDO

Pico FIDO adds passkeys, WebAuthn, U2F, OATH, and Yubico-style OTP features to a supported microcontroller board.

It uses the standard FIDO HID path, so browsers and most host software work with the protocol rather than the board identity. Yubico-oriented management tools are an exception because they may rely on reader names or VID/PID presets.

## Start here

- [Setup and policy](initialization.md): flashing, first validation, PIN, and early policy choices
- [Passkeys and CTAP](passkeys.md): resident credentials, algorithms, extensions, and reset behavior
- [Pico Vault](vault.md): enrolled passkey export/import and recovery
- [OATH accounts](accounts-oath.md): TOTP and HOTP accounts
- [OTP slots](slots-otp.md): static password, HOTP, Yubico OTP, and challenge-response
- [Vendor extensions](vendor-extensions.md): admin/user PIN split, permission masks, and app-level management paths

## What it supports

Upstream claims support for:

- CTAP 2.1 and CTAP 1
- FIDO 2.3 behavior and related authenticator configuration
- WebAuthn and U2F
- discoverable credentials and credential management
- user verification by PIN
- `hmac-secret`, `credProtect`, `minPinLength`, `credBlob`, and large-blob related extensions
- OATH accounts
- Yubico-style OTP slots
- self attestation and enterprise attestation
- backup with 24 words
- secure lock, secure boot, rescue paths, and OTP-backed secrets on stronger hardware families

Pico FIDO 8 also adds per-credential expiration and revocation, configurable
PIN complexity, `alwaysUv` and `makeCredUvNotRqd` policies, and the opt-in Pico
Vault portability workflow.

The feature set is broad, although host tools expose different parts of it.

## What it is not

Pico FIDO is not:

- an OpenPGP smart card
- a general PKCS#11 token
- a compact HSM API
- a secure element

For smart-card workflows, see [Pico OpenPGP](../pico-openpgp/index.md). For a broader cryptographic appliance, see [Pico HSM](../picohsm/index.md).

## Hardware matters

The upstream security story is not uniform across boards.

- RP2040 does not offer the same at-rest security properties as RP2350/RP2354 or ESP32-S3-class hardware.
- Secure Boot, Secure Lock, and OTP-backed key protection are meaningful only on the platforms that actually implement them.
- A hostile host can still drive authorized operations while the device is connected and unlocked.

!!! warning
    Do not describe all Pico FIDO deployments as equally secure. The board choice changes the threat model materially.

## Practical use

Pico FIDO makes sense when you want:

- passkeys and standard WebAuthn login
- second-factor registrations that behave like a normal security key
- OATH or OTP compatibility for older services

Everyday use depends on the host tools and hardware you choose, so account for both as you set up each workflow.
