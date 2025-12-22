# Troubleshooting

## Device not detected
Try in this order:
1. Use a different USB port (direct port preferred)
2. Use a different USB cable (data-capable)
3. Close other tools that might access the device
4. Reboot the computer (yes, sometimes it helps)

### Windows
- Check Device Manager for USB device entries.
- If you see driver errors, reinstall the app and reconnect the device.

### macOS
- If the app is blocked, allow it in **Privacy & Security**.
- Try a different port or hub.

### Linux
- USB permissions may block access.
- Run from terminal and look for permission errors.

## App opens but features are missing
- Some features depend on the firmware variant installed on the device.
- Some features may require registration.

## Firmware update fails
- Do not unplug immediately.
- Reopen PicoKeyApp and try again if the app indicates it is safe.
- If the device enters a boot/recovery mode, follow the recovery instructions provided with the firmware release.
