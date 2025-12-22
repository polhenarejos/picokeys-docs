# Troubleshooting

This section lists the most common issues encountered when using **PicoKeyApp**, together with their causes and recommended actions.

Read this section carefully before opening a support ticket.

---

## Device not detected

PicoKeyApp does not detect any connected device.

### Possible causes

- The device is not connected correctly
- The USB cable does not support data
- Another application is accessing the device
- USB permissions are missing (Linux)
- The device is in an unexpected mode

### Recommended actions

- Use a direct USB port on the computer (avoid hubs)
- Try a different USB cable
- Close other applications that may access USB devices
- Disconnect and reconnect the device
- Restart PicoKeyApp

!!! tip
    For first-time setup, always use a direct USB connection without hubs.

---

## Device detected but in unexpected connection mode

The device is detected, but the **Connection** field shows an unexpected value.

### Explanation

Depending on the firmware and device state, the connection mode may vary.

Common modes include:

- `RESCUE` (BOOTSEL / recovery mode)
- Normal operation mode (firmware-dependent)

### What to do

- For new or freshly flashed devices, `RESCUE` mode is expected
- If the device should be in normal mode, try reconnecting it
- If unsure, proceed with commissioning only when instructed

!!! note
    Connection mode alone does not indicate a faulty device.

---

## License installed but board is not registered

A license is installed, but the board status remains **Not registered**.

### Explanation

Installing a license and registering a board are **two separate steps**.

A license must be installed first, and then explicitly bound to a board.

### Recommended actions

- Go to **About → License**
- Verify that the license is correctly installed
- Select the correct board type
- Click **Register Board**

!!! warning
    Board registration is a required step and cannot be skipped.

---

## Selected the wrong board type

The wrong board type was selected during registration.

### Explanation

Board type selection is **user-controlled** and **irreversible**.

Once a board is registered, the selected board type **cannot be changed**.

!!! danger
    Selecting an incorrect board type cannot be undone.

    The license will remain bound to the selected board type permanently.

### What to do

- Do **not** attempt to work around this by selecting a similar board
- Do **not** attempt to re-register the board

Instead, open a support ticket and explain the situation.

---

## My board does not appear in the board list

The board model you are using does not appear in the board selection list.

### Recommended actions

- Do not proceed with board registration
- Do not select a different or similar board

!!! danger
    Registering a board with an incorrect board type may permanently bind the license incorrectly.

### Next step

Open a ticket at:

[https://github.com/polhenarejos/picokeyapp/issues](https://github.com/polhenarejos/picokeyapp/issues)

Include:

- The exact board model
- PicoKeyApp version
- Firmware version
- Any relevant screenshots or logs

---

## Firmware update fails

A firmware update does not complete successfully.

### Possible causes

- USB connection interrupted
- Device disconnected during update
- Incorrect firmware file
- Device not in the expected mode

### Recommended actions

- Do not unplug the device immediately
- Follow any instructions shown by PicoKeyApp
- If the device reappears in `RESCUE` mode, retry the update
- Use a direct USB connection

!!! warning
    Interrupting a firmware update may require recovery steps.

---

## Features missing after firmware update

Some features are missing after installing or updating firmware.

### Explanation

Available features depend on the **installed firmware**, not on PicoKeyApp itself.

Installing a different firmware variant may:

- Enable new features
- Disable features available in a previous firmware
- Require re-commissioning

### Recommended actions

- Verify the installed firmware version
- Review the documentation for the selected firmware variant
- Re-run commissioning if required

!!! note
    PicoKeyApp only exposes features supported by the firmware.

---

## Permission issues (Linux)

The device is detected, but operations fail on Linux.

### Explanation

Linux systems may require additional USB permissions.

### Recommended actions

- Check udev rules
- Ensure the user belongs to the appropriate groups
- Restart the session after changing permissions

!!! tip
    Running PicoKeyApp from a terminal can help identify permission-related errors.

---

## When to open a support ticket

Open a support ticket if:

- The correct board model is not listed
- Firmware recovery is not possible
- The device behaves inconsistently after following this guide

Use the official issue tracker:

[https://github.com/polhenarejos/picokeyapp/issues](https://github.com/polhenarejos/picokeyapp/issues)

Provide as much detail as possible to speed up resolution.
