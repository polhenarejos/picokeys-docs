# Key management

Pico OpenPGP follows the usual OpenPGP card model: signature, decryption, and authentication roles, each backed by on-device key material.

## On-card generation

The normal workflow is:

1. enter an admin-capable session
2. choose the desired attributes
3. generate or import the keys
4. verify that the resulting card state matches expectations

The upstream project lists support for:

- RSA generation from 1024 to 4096 bits
- ECDSA generation from 192 to 521 bits
- common NIST curves, Brainpool curves, and `secp256k1`

Those are firmware capabilities; the client experience still depends on what GnuPG and other host tools expose.

## Import vs generate

Choose between generation and import based on how you manage private keys:

- generate on-card when you want the private key never to have existed elsewhere
- import when you already have a controlled key hierarchy or recovery plan

These choices have different recovery and key-custody consequences, so document which one your workflow uses.

## Certificates and metadata

The upstream project also lists certificate handling and cardholder data support, although host interfaces expose those capabilities unevenly.

Validate the exact client workflow you intend to support before documenting it as smooth or routine.
