# Pico FIDO

The FIDO half of the PicoKeys family: passkeys, WebAuthn, U2F, OATH, and Yubico-style OTP features on a supported microcontroller board.

It speaks the standard FIDO HID path, so browsers and host software usually care more about the protocol surface than about the board identity. The exceptions are the Yubico-oriented management tools that key off reader names, VID/PID presets, or both.

## Start here

- [Setup and policy](initialization.md): flashing, first validation, PIN, and early policy choices
- [Passkeys and CTAP](passkeys.md): resident credentials, algorithms, extensions, and reset behavior
- [OATH accounts](accounts-oath.md): TOTP and HOTP accounts
- [OTP slots](slots-otp.md): static password, HOTP, Yubico OTP, and challenge-response
- [Vendor extensions](vendor-extensions.md): admin/user PIN split, permission masks, and app-level management paths

## What it supports

Upstream claims support for:

- CTAP 2.1 and CTAP 1
- WebAuthn and U2F
- discoverable credentials and credential management
- user verification by PIN
- `hmac-secret`, `credProtect`, `minPinLength`, `credBlob`, and large-blob related extensions
- OATH accounts
- Yubico-style OTP slots
- self attestation and enterprise attestation
- backup with 24 words
- secure lock, secure boot, rescue paths, and OTP-backed secrets on stronger hardware families

That is a large surface. It does not mean every host tool exposes every feature equally well.

## What it is not

Pico FIDO is not:

- an OpenPGP smart card
- a general PKCS#11 token
- a compact HSM API
- a secure element

If you need smart-card workflows, use [Pico OpenPGP](../pico-openpgp/index.md). If you need a broader cryptographic appliance, use [Pico HSM](../picohsm/index.md).

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

The documentation should therefore look like an operational guide, not a generic feature list. That is the intent of the pages in this section.
