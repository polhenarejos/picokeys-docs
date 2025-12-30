# Core concepts

This section explains the **core concepts** used throughout the PicoKeys ecosystem.

Understanding these concepts is essential to correctly use PicoKey App and the different firmware variants.

---

## PicoKey App vs firmware

The PicoKeys ecosystem is split into two distinct layers.

### PicoKey App (desktop application)

PicoKey App is the **management interface**.

It is responsible for:

- Detecting devices
- Displaying device and firmware information
- Configuring and commissioning devices
- Installing licenses and registering boards
- Providing diagnostics and troubleshooting tools

!!! note
    PicoKey App does not implement cryptographic functionality.

---

### Firmware (device functionality)

The firmware installed on the device defines **what the device actually does**.

Different firmware variants provide different functionality, such as:

- Hardware security module features (Pico HSM)
- FIDO2 / WebAuthn authentication (Pico FIDO)
- OpenPGP smartcard functionality (Pico OpenPGP)

!!! note
    The same PicoKey App interface is used regardless of the installed firmware.

---

## Device, board, and firmware

Several terms are used to describe different aspects of the hardware.

### Device

A **device** refers to the physical PicoKeys unit connected via USB.

Each device has:

- A unique serial number
- A hardware platform
- A specific board model

---

### Board

A **board** describes the exact hardware variant.

Board information is used to:

- Apply correct configuration
- Bind a license to compatible hardware
- Ensure firmware compatibility

!!! danger
    Board selection is irreversible once the board is registered.

---

### Firmware

The **firmware** is the software running on the device.

Firmware determines:

- Supported features
- Available security options
- Operational behavior

Changing firmware may change device capabilities without changing the hardware.

---

## License vs board registration

Licensing involves **two distinct steps**.

### License installation

Installing a license means:

- The license data is stored locally
- No device is modified yet

!!! note
    Installing a license alone does not enable any device functionality.

---

### Board registration

Registering a board means:

- Binding a license to a specific board
- Making the license effective for that device

!!! danger
    Board registration is irreversible.

    Once a license is bound to a board, it cannot be transferred or modified.

---

## Commissioning

Commissioning is the process of **finalizing device setup**.

Commissioning typically:

- Applies configuration settings
- Writes persistent state to the device
- Prepares the device for normal operation

Commissioning is usually performed after:

- Firmware installation
- Board registration

!!! warning
    Commissioning modifies persistent device state.

---

## Connection modes

A PicoKeys device can appear in different connection modes.

### RESCUE / BOOTSEL mode

This mode is typically used for:

- Initial setup
- Firmware updates
- Recovery operations

!!! note
    For new or freshly flashed devices, RESCUE mode is expected.

---

### Normal operation mode

In normal operation mode:

- The device runs the installed firmware
- Features exposed depend on the firmware variant

---

## Irreversible actions

Some actions in the PicoKeys ecosystem **cannot be undone**.

Examples include:

- Selecting and registering a board type
- Binding a license to a board
- Certain commissioning operations

!!! danger
    Always double-check irreversible actions before confirming.

---

## Responsibility model

The PicoKeys ecosystem follows a **user-controlled model**.

This means:

- The user explicitly selects actions
- PicoKey App does not auto-correct critical decisions
- Irreversible operations require explicit confirmation

!!! note
    This model is intentional and prioritizes transparency and control over automation.

---

## Summary

To use PicoKeys devices correctly, remember:

- PicoKey App is the management interface
- Firmware defines actual device functionality
- Board selection and registration are irreversible
- Commissioning finalizes device setup
- Understanding these concepts prevents configuration errors
