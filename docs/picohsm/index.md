# Pico HSM

Pico HSM is the broadest and most ambitious firmware in the PicoKeys family. It is meant to turn a supported board into a compact hardware security module with smart-card style transport and a large cryptographic surface.

That makes documentation quality more important, not less. A long feature list without boundaries is not useful.

## Start here

- [First steps](first-steps.md): detect the device, verify middleware, and establish a safe baseline
- [Capability map](features.md): what upstream says the firmware can do, grouped by function
- [Key generation](key-generation.md): create keys without exporting private material
- [Sign and verify](sign-verify.md): signature workflows
- [Asymmetric ciphering](asymmetric-ciphering.md): RSA decryption and ECDH
- [AES](aes.md): symmetric operations
- [Backup and restore](backup-restore.md): wrapped-key workflows and DKEK assumptions

## What it is, plainly

Pico HSM is not just "a device that can sign." Upstream claims support for:

- RSA, ECC, EdDSA, AES, HMAC, CMAC, KDFs, and secure messaging
- wrapped-key import/export through DKEK-based workflows
- key domains, threshold shares, attestation, counters, and public-key authentication
- USB CCID transport and PKCS#11 integration

This is closer to an HSM-style appliance than the other firmware families.

## What it is not

It is not:

- a certified enterprise HSM
- a secure element
- a guarantee that every advertised capability is equally well surfaced by generic desktop tooling

There is real capability here, but host-tool maturity varies sharply by feature.

## Security posture

The upstream README describes a model where:

- private and secret keys are stored encrypted in flash
- the MKEK protects that storage
- PIN-derived material gates key use
- RP2350/RP2354 and ESP32-S3 class hardware can back that model with stronger secure-boot, secure-lock, and OTP-backed protection

The boundary is just as important:

- a compromised host can still drive authorized operations
- RP2040 does not offer the same at-rest protection story
- physical and certification-grade threats are outside what these boards can honestly claim to solve

## How to read the rest of this section

The practical order is:

1. establish host connectivity and PIN workflow
2. generate or import only non-critical test material first
3. validate the exact host tools you plan to use
4. only then move on to backup, wrapped-key, or higher-assurance workflows

That order avoids confusing "feature exists" with "feature is operationally ready for this deployment."
