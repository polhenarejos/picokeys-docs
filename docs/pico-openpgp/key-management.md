# Key management

Pico OpenPGP follows the usual OpenPGP card model: signature, decryption, and authentication roles, each backed by on-device key material.

## On-card generation

The normal workflow is:

1. enter an admin-capable session
2. choose the desired attributes
3. generate or import the keys
4. verify that the resulting card state matches expectations

Upstream claims support for:

- RSA generation from 1024 to 4096 bits
- ECDSA generation from 192 to 521 bits
- common NIST curves, Brainpool curves, and `secp256k1`

That is the firmware claim. The practical client experience still depends on what GnuPG or other tooling exposes cleanly.

## Import vs generate

The usual trade-off remains:

- generate on-card when you want the private key never to have existed elsewhere
- import when you already have a controlled key hierarchy or recovery plan

The documentation should not pretend those are the same operational decision.

## Certificates and metadata

Upstream also claims certificate handling and cardholder data support. The important operational point is that not every host UI exposes those capabilities equally well.

Validate the exact client workflow you intend to support before documenting it as smooth or routine.
