# Pico OpenPGP

Pico OpenPGP runs a full OpenPGP smart-card applet over USB CCID. It provides three key roles and on-device cryptographic operations, with host-side tools such as GnuPG and OpenSC.

The firmware implements OpenPGP card 3.4.1, and the host stack must support the commands you plan to use.

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
- optional admin-less operation, where the user PIN can also authorize the
  administrative context when the firmware policy enables it

These capabilities fall into two groups:

- implemented in firmware
- realistically exposed by mainstream host software

## What it is not

Pico OpenPGP is not:

- a FIDO authenticator
- a general HSM appliance with Pico HSM's wider feature surface
- proof that every OpenPGP or PKCS#11 client supports every advertised card command

For broader cryptographic primitives, key domains, or wrapped-key workflows, see [Pico HSM](../picohsm/index.md).

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

The usual workflow is straightforward:

- card visible through PC/SC
- GnuPG or OpenSC talks to it
- on-device keys sign, decrypt, or authenticate


## Recent firmware changes

Recent firmware work adds migration-aware authenticated storage for OpenPGP
and PIV key material, stricter PIN and APDU validation, and more complete PIV
metadata and attestation handling. The PIV applet reports version 5.7 in the
current source tree.

For PIV, current defaults are deliberately role-specific: signature use
defaults to PIN verification, card-authentication keys can be PIN-free, and
generated/imported keys default to no touch unless a touch policy is supplied.
Review the reported slot metadata rather than assuming every slot has the same
policy.
