# PicoKeys Documentation

This site focuses first on the PicoKeys firmware families, then on the desktop application used to manage them.

If you are choosing firmware, start with the firmware sections below. If you already manage a device through the desktop app, use its pages as an operational reference; the firmware documentation defines each product's capabilities.

## Start here

- [Pico FIDO](picofido/index.md): passkeys, FIDO2, U2F, OATH, OTP slots, and vendor extensions
- [Pico OpenPGP](pico-openpgp/index.md): USB CCID smart card workflows for GnuPG, OpenSC, and PKCS#11
- [Pico HSM](picohsm/index.md): key generation, signing, decryption, symmetric cryptography, and wrapped-key workflows

## The ecosystem, plainly

PicoKeys is a family of firmwares for supported microcontroller boards, with each firmware defining a different device role.

The responsibilities are split across three layers:

- The firmware defines the real protocol surface, storage model, and security properties.
- The desktop application helps with flashing, commissioning, policy changes, and diagnostics.
- Host-side compatibility still depends on the protocol and middleware used by each firmware family.

Host compatibility also depends on the protocol and middleware each firmware uses:

- FIDO behavior is driven by what the authenticator reports through CTAP and what browsers or host tools actually consume.
- OpenPGP and HSM behavior depend heavily on CCID, PC/SC, OpenSC, PKCS#11, and client-specific support.
- RP2040, RP2350/RP2354, and ESP32-S3 do not offer the same hardware security baseline.

## Which firmware should you pick?

### Pico FIDO

Use it when the device is primarily an authenticator:

- passkeys and WebAuthn
- U2F / second-factor flows
- OATH accounts
- OTP slots and challenge-response

Pico FIDO is built for authentication. Choose another firmware if you need a general-purpose smart-card or HSM API.

### Pico OpenPGP

Use it when the device should behave like an OpenPGP smart card:

- GnuPG smart-card workflows
- on-device signing and decryption
- CCID / APDU / PKCS#11 style integration

Choose another firmware for FIDO2/WebAuthn or broad symmetric-key and HSM workflows.

### Pico HSM

Use it when the device is meant to act as a compact HSM:

- private-key generation and retention
- signatures and decryption through PKCS#11
- AES, HMAC, KDF, wrapped-key import/export, and advanced key domains

Its feature set is broad, while its hardware and assurance model remain those of supported microcontroller boards; it is not a certified enterprise HSM.

## Security baseline

Across the firmware families, the most important practical distinction is hardware:

- RP2040 boards do not provide the same hardware-backed at-rest protection described for RP2350/RP2354 and ESP32-S3 class devices.
- Secure Boot, Secure Lock, and OTP-backed key material are meaningful only on the platforms that actually implement those mechanisms.
- A host compromise can still drive operations on an unlocked device, regardless of firmware family.

These projects do not claim protection against well-funded physical attackers or certification-level requirements unless a specific capability has been demonstrated.

## Production hardening

Production hardening applies across products, and the right procedure depends on the board family, firmware image, and provisioning state.

Read these pages before burning irreversible state:

- [OTP fuses](production/otp-fuses.md): which RP2350/RP2354 OTP rows and ESP32-S3 eFuses are used for PicoKeys root keys, secure boot, and secure lock.
- [Anti-rollback](production/anti-rollback.md): why old signed firmware can still be dangerous, and what rollback floors are meant to prevent.
- [Threat model](security/threat-model.md): what PicoKeys protects against, what remains host-dependent, and what is out of scope.

!!! warning
    Do not enable secure boot, secure lock, rollback, or fuse-based protection as a casual setup step. These are production decisions with irreversible hardware effects.

## How to read the docs

The firmware sections follow the same approach:

- what the firmware is for
- what upstream claims it implements
- what host tooling is required
- what is easy, what is awkward, and what is unsupported
- where the sharp edges are

This structure keeps the focus on how each firmware works in practice, including the host tools it needs and the limits you may encounter.
