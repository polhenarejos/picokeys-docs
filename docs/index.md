# PicoKeys Documentation

This documentation covers the **PicoKeys ecosystem**, including the desktop application and the different firmware variants available for PicoKeys devices.

The PicoKeys ecosystem is composed of:

- A **desktop application** used to manage devices
- One or more **firmware variants** that define the actual functionality of the device

This documentation is intended for users who want to understand how to set up, manage, and use PicoKeys devices correctly.

---

## Architecture overview

The PicoKeys ecosystem is split into two clearly separated layers:

### 1. PicoKeyApp (desktop application)

**PicoKeyApp** is the desktop application used to manage PicoKeys devices.

It is responsible for:

- Device detection
- Device configuration and commissioning
- License installation and board registration
- Diagnostics and troubleshooting

PicoKeyApp provides the **same user interface regardless of the installed firmware**.

➡️ See:

- [PicoKeyApp](picokeyapp/index.md)
- [First steps](picokeyapp/first-steps.md)
- [Quick start](picokeyapp/quickstart.md)

---

### 2. Firmware variants (device functionality)

The actual behavior and capabilities of a PicoKeys device are defined by the **firmware installed on the device**.

Each firmware variant provides different functionality, while sharing the same hardware and management interface.

Available firmware variants include:

- **Pico HSM**
  A hardware security module firmware focused on secure key storage and cryptographic operations.

- **Pico FIDO**
  A FIDO2 / WebAuthn firmware for passwordless and second-factor authentication.

- **Pico OpenPGP**
  An OpenPGP-compatible smartcard firmware for encryption, signing, and authentication workflows.

➡️ See:

- [Pico HSM](picohsm/index.md)
- [Pico FIDO](picofido/index.md)
- [Pico OpenPGP](pico-openpgp/index.md)

---

## Typical usage flow

For most users, the typical workflow is:

1. Install PicoKeyApp
2. Install a license
3. Connect a PicoKeys device
4. Select and register the correct board type
5. Commission the device
6. Use the device according to the installed firmware
7. Select and flash the proper firmware

The first-time setup includes **mandatory and irreversible steps**.
Make sure to read the first-time use documentation before proceeding.

➡️ See:

- [First steps](picokeyapp/first-steps.md)

---

## Scope of this documentation

This documentation covers:

- How to use PicoKeyApp
- How to perform first-time setup safely
- What functionality is provided by each firmware variant
- Common concepts shared across the PicoKeys ecosystem

This documentation does **not** cover:

- Internal firmware implementation details
- Cryptographic theory
- Third-party tools documentation (e.g. GPG, browser-specific WebAuthn details)

---

## Getting help

If you encounter an issue with PicoKeyApp or the documentation:

- Check the relevant section in this documentation
- Review the troubleshooting information
- If the issue persists, open a ticket at:

[https://github.com/polhenarejos/picokeyapp/issues](https://github.com/polhenarejos/picokeyapp/issues)
