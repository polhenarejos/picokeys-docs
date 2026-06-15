# Pico OpenPGP

A full OpenPGP smart card applet over USB CCID: three key roles, host-side GnuPG and OpenSC tooling, and on-device cryptographic operations without exporting private key material.

The important part is not just that the firmware speaks OpenPGP card 3.4.1. It is that the host stack must speak it correctly too.

## Start here

- [Host setup and compatibility](compatibility-notes.md): CCID, PC/SC, OpenSC, GnuPG, and where detection usually fails
- [PINs and roles](pin-and-roles.md): `PW1`, `PW3`, reset code, and role boundaries
- [Operational limits](operational-limits.md): slow RSA, uneven client support, and why firmware support is not enough

## What it supports

Upstream claims support for:

- OpenPGP card 3.4.1 style operation
- key generation and encrypted storage
- RSA from 1024 to 4096 bits
- ECDSA from 192 to 521 bits
- `secp256r1`, `secp384r1`, `secp521r1`, `brainpoolP256r1`, `brainpoolP384r1`, `brainpoolP512r1`, and `secp256k1`
- RSA signatures, ECDSA signatures, and ECDH
- KDF for PIN, UIF, MSE, card lifecycle operations, certificates, extended APDU, and AES encipher/decipher support

That is a broad feature set. It still needs to be separated into two buckets:

- implemented in firmware
- realistically exposed by mainstream host software

## What it is not

Pico OpenPGP is not:

- a FIDO authenticator
- a general HSM appliance with Pico HSM's wider feature surface
- proof that every OpenPGP or PKCS#11 client supports every advertised card command

If you need broader cryptographic primitives, key domains, or wrapped-key workflows, [Pico HSM](../picohsm/index.md) is the better fit.

## Security posture

The upstream README describes:

- encrypted storage using a Device Encryption Key
- PIN never stored as plain text
- stronger hardware-backed protection on RP2350/RP2354 and ESP32-S3 class boards

The limits are just as important:

- RP2040 does not provide the same at-rest story
- a hostile host can still drive authorized card operations
- client support for advanced features is uneven even when the firmware implements them

## Practical expectation

The default use case is still the familiar OpenPGP card workflow:

- card visible through PC/SC
- GnuPG or OpenSC talks to it
- on-device keys sign, decrypt, or authenticate

These docs now follow that flow much more directly.
