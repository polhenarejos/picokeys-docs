# Troubleshooting

Use this guide to check common PicoKey App problems before opening a support ticket.

---

## Device not detected

PicoKey App does not detect the connected device.

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
- Restart PicoKey App

!!! tip
    For first-time setup, always use a direct USB connection without hubs.

---

## Device detected but in unexpected connection mode

The device is detected, but the **Connection** field shows an unexpected value.

### Explanation

The connection mode depends on the firmware and device state.

Common modes include:

- `RESCUE` (BOOTSEL / recovery mode)
- Normal operation mode (firmware-dependent)

### What to do

- A new or freshly flashed device is expected to be in `RESCUE` mode.
- If the device should be in normal mode, reconnect it and check again.
- If you are unsure, commission the device only when instructed.

!!! note
    Connection mode alone does not indicate a faulty device.

---

## License installed but board is not registered

A license is installed, but the board status remains **Not registered**.

### Explanation

Installing a license and registering a board are **separate steps**. Install
the license first, then bind it to the selected board.

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

Board type selection is **user-controlled** and **irreversible**. Once the
board is registered, you cannot change the selected type.

!!! danger
    Selecting an incorrect board type cannot be undone.

    The license will remain bound to the selected board type permanently.

### What to do

- Do **not** select a similar board as a workaround.
- Do **not** try to register the board again.

Instead, open a support ticket and explain the situation.

---

## My board does not appear in the board list

The board selection list does not include your board model.

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
- PicoKey App version
- Firmware version
- Any relevant screenshots or logs

---

## Firmware update fails

The firmware update does not complete successfully.

### Possible causes

- USB connection interrupted
- Device disconnected during update
- Incorrect firmware file
- Device not in the expected mode

### Recommended actions

- Do not unplug the device immediately
- Follow any instructions shown by PicoKey App
- If the device reappears in `RESCUE` mode, retry the update
- Use a direct USB connection

!!! warning
    Interrupting a firmware update may require recovery steps.

---

## Features missing after firmware update

Some features are missing after installing or updating firmware.

### Explanation

Available features depend on the **installed firmware** rather than PicoKey App.

Installing another firmware variant may:

- Enable new features
- Disable features available in a previous firmware
- Require re-commissioning

### Recommended actions

- Verify the installed firmware version
- Review the documentation for the selected firmware variant
- Re-run commissioning if required

!!! note
    PicoKey App only exposes features supported by the firmware.

---

## Permission issues (Linux)

The device is detected, but operations fail on Linux.

### Explanation

Linux may require additional USB permissions before the device can be used.

### Recommended actions

- Check udev rules
- Ensure the user belongs to the appropriate groups
- Restart the session after changing permissions

!!! tip
    Running PicoKey App from a terminal can help identify permission-related errors.

---

## When to open a support ticket

Open a support ticket when:

- The correct board model is not listed
- Firmware recovery is not possible
- The device behaves inconsistently after following this guide

Use the official issue tracker:

[https://github.com/polhenarejos/picokeyapp/issues](https://github.com/polhenarejos/picokeyapp/issues)

Include the details above so the support team can investigate the issue.
