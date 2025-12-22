# Pico OpenPGP

This document summarizes the purpose and capabilities of the **Pico OpenPGP** firmware, based directly on the official project README.

Pico OpenPGP transforms compatible hardware — such as a Raspberry Pico or ESP32 microcontroller — into a **USB smart card with an integrated OpenPGP applet**.

---

## Purpose

Pico OpenPGP is designed to:

- Provide a hardware-backed OpenPGP smart card implementation

- Run the OpenPGP applet in a USB-CCID smart card environment

- Offer secure key storage and cryptographic operations for OpenPGP keys

!!! note
    The firmware aims to follow the OpenPGP 3.4.1 specification as used in GnuPG.

---

## What Pico OpenPGP is

Pico OpenPGP is:

- A firmware for converting appropriate microcontrollers into an OpenPGP smart card

- An implementation of the OpenPGP protocol in a CCID smart card context

- A tool for managing PGP keys securely on hardware

---

## What Pico OpenPGP is not

Pico OpenPGP is not:

- A general-purpose hardware security module (HSM)

- A FIDO2/WebAuthn authenticator (use **Pico FIDO** for that)

- A software-only OpenPGP library

!!!

---

## Supported workflows

With Pico OpenPGP, you can perform typical smart card operations such as:

- Generating OpenPGP keys on the device

- Signing data with private OpenPGP keys

- Decrypting data using private OpenPGP keys

- Managing certificates and key attributes via OpenPGP interfaces

!!! note
    The exact operations available depend on the OpenPGP applet implementation and the connected host tools.

---

## Interfaces

Pico OpenPGP exposes itself as a **CCID smart card device** over USB.

This enables compatibility with:

- Smart card middleware

- PKCS#11 interfaces

- GnuPG (`gpg`)

- Other OpenPGP-aware software

!!! warning
    Host support for CCID and smart card tools must be configured correctly to interact with the device.

---

## Hardware support

Pico OpenPGP runs on:

- Raspberry Pico boards

- ESP32 series boards

!!! note
    The exact list of supported boards may vary with firmware releases.

---

## Smart card behavior

As a USB smart card, Pico OpenPGP behaves like:

- A physical smart card implementing the OpenPGP applet

- A device that responds to APDU commands for OpenPGP operations

---

## Typical use cases

Common scenarios for Pico OpenPGP include:

- Signing email messages and documents cryptographically

- Decrypting OpenPGP-encrypted content

- Managing OpenPGP keys securely

!!! tip
    Using Pico OpenPGP with GnuPG provides a secure workflow for signing and encrypting email and files.

---

## Security considerations

When using Pico OpenPGP:

- Protect access to the smart card PIN

- Avoid exposing private keys outside the hardware

- Use trusted host software (e.g., GnuPG)

!!! danger
    Compromise of the host machine could undermine the security of the OpenPGP workflow.

---

## Summary

Pico OpenPGP provides an **open-source implementation** of an OpenPGP smart card applet on small microcontrollers, with secure key storage and cryptographic operations compatible with standard smart card tools.
