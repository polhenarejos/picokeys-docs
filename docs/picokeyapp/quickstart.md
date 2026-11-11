# Quick start

Follow this **first-time workflow** to connect and commission a device with
PicoKey App. It starts from a device with no prior configuration.

---

## Step 1: Connect the device

1. Plug the PicoKeys device into a USB port.
2. Use a direct USB connection whenever possible (avoid hubs during initial setup).
3. Launch **PicoKey App**.

PicoKey App should detect the device within a few seconds.

---

## Step 2: Verify device detection

Go to **Home**.

When detection succeeds, the green **Device detected** indicator appears with details such as:

  - Platform (e.g. RP2350)
  - Firmware version
  - Serial number
  - Board model
  - Board status (Registered / Not registered)

If the device is **not detected**:

- Try a different USB port or cable
- Close other applications that might be accessing the device
- Reconnect the device and restart PicoKey App

---

## Step 3: Check connection mode

In the **Home** section, check the **Connection** field.

Typical values include:

- `RESCUE`: the device is in recovery or commissioning mode.
- Other values depending on the installed firmware

For a new or freshly flashed device, `RESCUE` mode is expected.

---

## Step 4: Configure the device

Go to **Configuration**.

Review the available options:

- USB identity (Vendor preset, VID:PID, Product string)
- Timeouts and LED behavior
- Advanced options (if supported)

Review these options and change any settings you need.

!!! Note
    Available options depend on the installed firmware.
    Unsupported options may have no effect.

---

## Step 5: Commission the device

Still in **Configuration**, click **Commission device**.

Commissioning applies your selections, provisions the device, and writes persistent state to it.

- Applies the selected configuration
- Provisions the device
- Writes persistent state to the device

Wait for a success message, such as *Commissioned successfully*.

!!! Warning
    Commissioning modifies persistent device state.
    Ensure all settings are correct before proceeding.

---

## Step 6: (Optional) Register a license

Go to **About → License**.

If the device is not registered:

1. Click **Register License**
2. Enter or load your license information
3. If required, select the target board
4. Click **Register Board**

Once completed, the board status should change to **Registered**.

---

## Step 7: Verify final status

Return to **Home** and verify:

- Device is detected
- Board status is correct
- No error messages are displayed

The device is now ready to use with its installed firmware.

---

## What’s next?

PicoKey App presents the same interface across firmware variants. Choose the guide for your installed firmware:

- **Pico HSM:** cryptographic operations and PKCS#11 usage.
- **Pico FIDO:** FIDO2 and WebAuthn authentication.
- **Pico OpenPGP:** OpenPGP smart-card workflows.
