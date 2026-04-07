# Pico OpenPGP

Pico OpenPGP turns supported microcontrollers into a USB smart card running an OpenPGP applet.

It is designed to follow the OpenPGP card 3.4.1 model used by common smart-card tooling.

---

## Scope

Pico OpenPGP is intended for:

- Hardware-backed OpenPGP key storage
- On-device key generation
- Signing and asymmetric decryption workflows
- Smart-card style management through APDU/CCID interfaces

It is not intended to replace:

- Pico HSM (general HSM workflows)
- Pico FIDO (WebAuthn/FIDO2 flows)

---

## Standards and interfaces

Pico OpenPGP exposes a USB CCID smart-card interface and is designed to work with:

- OpenSC middleware/tooling
- PKCS#11-capable applications (`pkcs11-tool`, OpenSSL engines, similar)
- GnuPG smart-card workflows (`gpg --edit-card --expert`)
- PKCS#15 tooling (`pkcs15-tool`)

!!! note
    Host-side PC/SC and CCID configuration is required for successful detection and operation.

---

## Implemented feature highlights

Based on the official project README, Pico OpenPGP includes:

- Key generation and encrypted key storage
- RSA key generation (1024 to 4096 bits)
- ECDSA key generation (192 to 521 bits)
- ECC curves:
  - `secp256r1`, `secp384r1`, `secp521r1`
  - `brainpoolP256r1`, `brainpoolP384r1`, `brainpoolP512r1`
  - `secp256k1`
- Digests: `SHA1`, `SHA224`, `SHA256`, `SHA384`, `SHA512`
- RSA signature support (PKCS and raw)
- ECDSA signature support (raw and hash)
- ECDH key derivation
- PIN authorization and KDF for PIN
- User Interaction Flag (UIF) / press-to-confirm control
- Manage Security Environment (MSE)
- Card lifecycle operations (activation/termination)
- Extended APDU support
- Cardholder certificate support
- USB/CCID support with smart-card toolchains

---

## AES support

Pico OpenPGP also implements AES paths described in OpenPGP card flows (PSO encipher/decipher commands), including AES key generation.

The project notes that broad off-the-shelf OpenPGP software support for these AES paths is limited, so these operations are typically used through specialized tooling, PKCS#11 integration, or direct APDU workflows.

---

## Security architecture notes

From the project documentation:

- Sensitive key material is stored encrypted using a Device Encryption Key (DEK).
- PIN is not stored as plain text in flash.
- Additional hardening is available on platforms with stronger secure features (for example RP2350 / ESP32-S3 secure boot and secure lock capabilities).
- OTP-backed storage is used for critical keying material in supported hardware profiles.

!!! warning
    Device security still depends on host security. If the host is compromised, user workflows can be at risk.

---

## Hardware and deployment

Typical targets include Raspberry Pi Pico family and ESP32-S3 based boards.

Deployment options include:

- Flashing prebuilt firmware releases
- Building from source with custom board and VID/PID settings
- Commissioning and management through PicoKey App

---

## Practical usage in this docs set

For UI-level operations, see PicoKey App OpenPGP pages:

- [OpenPGP Management](../picokeyapp/openpgp/management.md)
- [PIV](../picokeyapp/openpgp/piv.md)

For related firmware families:

- [Pico FIDO](../picofido/index.md)
- [Pico HSM](../picohsm/index.md)
