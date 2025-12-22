# Installation

This page explains how to install PicoKeyApp on your operating system.

## Downloads
Download PicoKeyApp from the official downloads page:

- PicoKeyApp downloads: (add your link here)

!!! note
    Always download PicoKeyApp from official sources to avoid tampered binaries.

## Windows
1. Download the `.exe` installer (or portable build, depending on release).
2. Run the installer.
3. Launch **PicoKeyApp** from the Start menu.

### Windows driver notes
PicoKeyApp uses standard USB drivers where possible. If your device is not detected, go to:
- [Troubleshooting](troubleshooting.md)

## macOS
1. Download the `.dmg` (or `.zip`) build.
2. Drag **PicoKeyApp** into **Applications**.
3. Open PicoKeyApp.

!!! tip
    If macOS blocks the app, open **System Settings → Privacy & Security** and allow the application.

## Linux
1. Download the AppImage.
2. Make it executable:
   ```bash
   chmod +x PicoKeyApp.AppImage
   ```
3. Run it:
    ```bash
    ./PicoKeyApp.AppImage
    ```

!!! Linux permissions (USB access)
If the device is not detected, you may need udev rules or group permissions. See:
- [Troubleshooting](troubleshooting.md)
