# Core concepts

Before configuring a device, it helps to understand how PicoKey App, firmware,
board registration, and commissioning fit together.

---

## PicoKey App vs firmware

PicoKeys has two layers: the desktop application and the firmware on the device.

### PicoKey App (desktop application)

PicoKey App is the **management interface**.

PicoKey App handles the management work:

- It detects devices and displays device and firmware information.
- It configures and commissions devices, and manages licenses and board registration.
- It also provides diagnostic and troubleshooting tools.

!!! note
    PicoKey App does not implement cryptographic functionality.

---

### Firmware (device functionality)

The installed firmware defines **what the device does**.

Each firmware variant provides a different set of functions, for example:

- Pico HSM provides hardware security module features.
- Pico FIDO provides FIDO2 and WebAuthn authentication.
- Pico OpenPGP provides OpenPGP smart-card functions.

!!! note
    The same PicoKey App interface is used regardless of the installed firmware.

---

## Device, board, and firmware

The words *device*, *board*, and *firmware* refer to different parts of the setup.

### Device

A **device** is the physical PicoKeys unit connected over USB.

It has:

- A unique serial number
- A hardware platform
- A specific board model

---

### Board

A **board** identifies the exact hardware variant.

PicoKey App uses this information to:

- Apply correct configuration
- Bind a license to compatible hardware
- Ensure firmware compatibility

!!! danger
    Board selection is irreversible once the board is registered.

---

### Firmware

The **firmware** is the software that runs on the device.

It determines:

- Supported features
- Available security options
- Operational behavior

Changing firmware may change device capabilities without changing the hardware.

---

## License vs board registration

Licensing has **two steps**, and installing the license does not register a board.

### License installation

When you install a license:

- The license data is stored locally
- No device is modified yet

!!! note
    Installing a license alone does not enable any device functionality.

---

### Board registration

When you register a board:

- Binding a license to a specific board
- Making the license effective for that device

!!! danger
    Board registration is irreversible.

    Once a license is bound to a board, it cannot be transferred or modified.

---

## Commissioning

Commissioning **finalizes device setup** by applying configuration and writing persistent state.

It typically:

- Applies configuration settings
- Writes persistent state to the device
- Prepares the device for normal operation

You normally commission the device after:

- Firmware installation
- Board registration

!!! warning
    Commissioning modifies persistent device state.

---

## Connection modes

A PicoKeys device can appear in different connection modes.

### RESCUE / BOOTSEL mode

Use this mode for:

- Initial setup
- Firmware updates
- Recovery operations

!!! note
    For new or freshly flashed devices, RESCUE mode is expected.

---

### Normal operation mode

In normal mode:

- The device runs the installed firmware
- Features exposed depend on the firmware variant

---

## Irreversible actions

Some PicoKeys actions **cannot be undone**. These include:

Examples include:

- Selecting and registering a board type
- Binding a license to a board
- Certain commissioning operations

!!! danger
    Always double-check irreversible actions before confirming.

---

## Responsibility model

PicoKeys follows a **user-controlled model**, where:

- The user explicitly selects actions
- PicoKey App does not auto-correct critical decisions
- Irreversible operations require explicit confirmation

!!! note
    This model prioritizes transparency and user control over automation.
