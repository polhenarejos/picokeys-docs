# Quick start

This guide walks through a **typical first-time workflow** using PicoKey App, from connecting a device to having it commissioned and ready for use.

No prior configuration is assumed.

---

## Step 1 – Connect the device

1. Plug the PicoKeys device into a USB port.
2. Use a direct USB connection whenever possible (avoid hubs during initial setup).
3. Launch **PicoKey App**.

After a few seconds, the application should detect the device automatically.

---

## Step 2 – Verify device detection

Go to **Home**.

If the device is detected correctly, you should see a green **Device detected** indicator and device information such as:

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

## Step 3 – Check connection mode

In the **Home** section, check the **Connection** field.

Typical values include:

- `RESCUE` – Device is in recovery / commissioning mode
- Other values depending on the installed firmware

For a new or freshly flashed device, `RESCUE` mode is expected.

---

## Step 4 – Configure the device

Go to **Configuration**.

Review the available options:

- USB identity (Vendor preset, VID:PID, Product string)
- Timeouts and LED behavior
- Advanced options (if supported)

Adjust settings as needed.

!!! Note
    Available options depend on the installed firmware.
    Unsupported options may have no effect.

---

## Step 5 – Commission the device

Still in **Configuration**, click **Commission device**.

This step:

- Applies the selected configuration
- Provisions the device
- Writes persistent state to the device

Wait for the confirmation message indicating success (for example, *PHY loaded successfully*).

!!! Warning
    Commissioning modifies persistent device state.
    Ensure all settings are correct before proceeding.

---

## Step 6 – (Optional) Register a license

Go to **About → License**.

If the device is not registered:

1. Click **Register License**
2. Enter or load your license information
3. If required, select the target board
4. Click **Register Board**

Once completed, the board status should change to **Registered**.

---

## Step 7 – Verify final status

Return to **Home** and verify:

- Device is detected
- Board status is correct
- No error messages are displayed

At this point, the device is ready to be used with the installed firmware.

---

## What’s next?

PicoKey App provides the same UI for different firmware variants.

Next steps depend on which firmware is installed:

- **Pico HSM** – Cryptographic operations and PKCS#11 usage
- **Pico FIDO** – FIDO2 / WebAuthn authentication
- **Pico OpenPGP** – OpenPGP smartcard workflows
