# Pico HSM

Pico HSM gives a supported board a broad cryptographic feature set and smart-card transport, making it the most HSM-oriented firmware in the PicoKeys family.

Because the feature set is broad, these pages explain where each capability applies and what host support it needs.

## Start here

- [First steps](first-steps.md): detect the device, verify middleware, and establish a safe baseline
- [Capability map](features.md): what upstream says the firmware can do, grouped by function
- [Key generation](key-generation.md): create keys without exporting private material
- [Sign and verify](sign-verify.md): signature workflows
- [Asymmetric ciphering](asymmetric-ciphering.md): RSA decryption and ECDH
- [AES](aes.md): symmetric operations
- [Backup and restore](backup-restore.md): wrapped-key workflows and DKEK assumptions

## What it supports

Upstream lists support for a broad range of cryptographic and management functions:

- RSA, ECC, EdDSA, AES, HMAC, CMAC, KDFs, and secure messaging
- wrapped-key import/export through DKEK-based workflows
- key domains, threshold shares, attestation, counters, and public-key authentication
- USB CCID transport and PKCS#11 integration

Together, these features make Pico HSM more appliance-like than the other firmware families.

## What it is not

It is not:

- a certified enterprise HSM
- a secure element
- a guarantee that every advertised capability is equally well surfaced by generic desktop tooling

Host-tool support varies by feature, so a firmware capability may still require a specific client or additional setup.

## Security posture

The upstream README describes a model where:

- private and secret keys are stored encrypted in flash
- the MKEK protects that storage
- PIN-derived material gates key use
- RP2350/RP2354 and ESP32-S3 class hardware can back that model with stronger secure-boot, secure-lock, and OTP-backed protection

The device still has these limits:

- a compromised host can still drive authorized operations
- RP2040 does not offer the same at-rest protection story
- physical and certification-grade threats are outside what these boards can honestly claim to solve

## Recommended evaluation order

The practical order is:

1. establish host connectivity and PIN workflow
2. generate or import only non-critical test material first
3. validate the exact host tools you plan to use
4. only then move on to backup, wrapped-key, or higher-assurance workflows

Following that order helps confirm that a feature works with the host tools and deployment you intend to use.

## Current storage behavior

Recent firmware work moves HSM key material toward authenticated SDK object
containers while retaining compatibility with supported legacy layouts. The
practical consequences are:

- firmware upgrades do not require treating every existing key as a new key;
- protected key records carry authenticated metadata and policy facts;
- authenticated PIN state is required before sensitive key-domain and key
  material changes; and
- the shared filesystem supports larger, multi-sector protected stores.

This improves migration and crash recovery, but it does not replace a tested
DKEK backup and restore procedure.
