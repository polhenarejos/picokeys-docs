# PicoKeys Documentation

This site documents the firmware families in the PicoKeys ecosystem first, and the management application second.

If you are here to decide which firmware to use, start with the firmware sections below. If you are already managing a device through the desktop app, keep the app pages as an operational reference, not as the source of truth for product capabilities.

## Start here

- [Pico FIDO](picofido/index.md): passkeys, FIDO2, U2F, OATH, OTP slots, and vendor extensions
- [Pico OpenPGP](pico-openpgp/index.md): USB CCID smart card workflows for GnuPG, OpenSC, and PKCS#11
- [Pico HSM](picohsm/index.md): key generation, signing, decryption, symmetric cryptography, and wrapped-key workflows

## The ecosystem, plainly

PicoKeys is not one product. It is a family of firmwares that run on supported microcontroller boards.

The split matters:

- The firmware defines the real protocol surface, storage model, and security properties.
- The desktop application helps with flashing, commissioning, policy changes, and diagnostics.
- Host-side compatibility still depends on the protocol and middleware used by each firmware family.

That last point is where many docs sets become vague. This one should not:

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

Do not pick Pico FIDO because you need a general-purpose smart card or HSM API. That is not its job.

### Pico OpenPGP

Use it when the device should behave like an OpenPGP smart card:

- GnuPG smart-card workflows
- on-device signing and decryption
- CCID / APDU / PKCS#11 style integration

Do not pick it for FIDO2/WebAuthn or broad symmetric/HSM workflows.

### Pico HSM

Use it when the device is meant to act as a compact HSM:

- private-key generation and retention
- signatures and decryption through PKCS#11
- AES, HMAC, KDF, wrapped-key import/export, and advanced key domains

Do not treat it as a certified enterprise HSM. The feature set is broad, but the hardware and assurance model remain those of supported microcontroller boards.

## Security baseline

Across the firmware families, the most important practical distinction is hardware:

- RP2040 boards do not provide the same hardware-backed at-rest protection described for RP2350/RP2354 and ESP32-S3 class devices.
- Secure Boot, Secure Lock, and OTP-backed key material are meaningful only on the platforms that actually implement those mechanisms.
- A host compromise can still drive operations on an unlocked device, regardless of firmware family.

If your threat model includes funded physical attackers or certification requirements, you should assume these projects are outside that scope unless explicitly proven otherwise.

## Production hardening

Production hardening is intentionally not a top-level product tab. It is a cross-cutting subject that depends on the board family, the firmware image, and the provisioning state.

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

That is intentional. Glossy feature lists are easy to write and often misleading. Operational documentation is more useful.
