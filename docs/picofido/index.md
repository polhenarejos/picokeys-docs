# Pico FIDO

This document summarizes the core features and capabilities of the **Pico FIDO** firmware based on the official project README.

---

## Purpose

Pico FIDO transforms supported hardware (such as Raspberry Pi Pico or ESP32) into a **FIDO Passkey authenticator** that operates like a standard USB security key.

Pico FIDO is designed for:

- WebAuthn / FIDO2 authentication
- U2F compatibility
- Strong user authentication without passwords
- Credential management

!!! note
    Pico FIDO does **not** implement general-purpose cryptographic APIs outside the FIDO2/WebAuthn protocol.

---

## Supported standards

Pico FIDO implements these authentication standards and extensions:

- CTAP 2.1 / CTAP 1 (FIDO2 protocols)
- WebAuthn (browser/OS authentication API)
- U2F (legacy FIDO support)

---

## Credential and authenticator features

Pico FIDO includes support for:

- HMAC-Secret extension
- CredProtect extension
- User presence enforcement (physical button)
- User verification with PIN
- Discoverable credentials (resident keys)
- Credential management (list, delete, etc.)

---

## Cryptographic algorithm support

Pico FIDO supports the following authentication key types and curves:

- ECDSA (various curves)
- EDDSA (when enabled)

Supported curves include:

- secp256r1
- secp384r1
- secp521r1
- secp256k1
- Ed25519

---

## Usage capabilities

Beyond basic FIDO2/WebAuthn flows, Pico FIDO adds:

- App registration and login support
- Device selection for multiple authenticators
- Support for vendor configuration parameters
- Backup with 24-word mnemonic phrase
- Secure Lock to protect against flash dumps

---

## Extension support

Pico FIDO implements various FIDO extensions:

- minPinLength extension
- Self-attestation
- Enterprise attestation
- credBlobs extension
- largeBlobKey and large blob support (up to 2048 bytes)

---

## OTP and challenge-response

Pico FIDO also provides:

- OATH support (based on YKOATH)
- TOTP / HOTP generation
- Yubikey-style One Time Password (OTP)
- Challenge-response generation

---

## Optional interfaces

Pico FIDO supports additional interfaces:

- Emulated keyboard interface for OTP entry

---

## Security and hardware features

Pico FIDO includes:

- Permissions system (MC, GA, CM, ACFG, LBW)
- Authenticator configuration UI support
- Backup mnemonic support

!!! warning
    Secure lock and certain configuration options may make the device permanently configured unless reset.

---

## Summary

Pico FIDO is a **full-featured FIDO2/WebAuthn authenticator** firmware for Raspberry Pico and ESP32 devices.

It supports modern extensions, credential management, backup/rescue flows, and a variety of cryptographic curves, making it compatible with most WebAuthn and CTAP2 ecosystems.
