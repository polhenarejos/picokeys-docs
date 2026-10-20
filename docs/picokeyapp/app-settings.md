# App settings

This section describes the **application-level settings** available in PicoKey App.
These settings affect how the application communicates with PicoKey devices and how certain advanced features are enabled.

![Application settings](../assets/images/picokeyapp/app-settings.png)

---

## Available readers

Selects the smart card / USB reader interface used by PicoKey App.

- The list shows all detected compatible interfaces
- The active reader is highlighted
- The refresh button forces a new scan of available readers

!!! note
    In most cases, the default reader selection is sufficient and should not be changed.

---

## Force Rescue Interface

Forces the application to use the **Rescue Interface** instead of the default PC/SC interface.

When enabled:

- PicoKey App bypasses the standard PC/SC stack
- Communication is performed through the Rescue Interface
- This may be required if PC/SC is misconfigured or unavailable

!!! warning
    The Rescue Interface provides reduced functionality compared to PC/SC.

---

## Patch PC/SC

Attempts to patch the local **pcscd service** to improve compatibility with PicoKey devices on some Linux distributions.

When enabled:

- PicoKey App applies changes to the PC/SC service
- Compatibility issues with certain distributions may be resolved

!!! danger
    This operation requires root privileges and may affect other smart card devices.
    Use with caution.

---

## Enable Notifications

Enables or disables the display or notifications when an event is processed by the device. Known events:

- A site is requesting your presence by touching the button.
- A credential has been created.
- An assertion for a specific credential has been succeed.

!!! note
    Notifications are sent to the Notification Center in macOS, Linux and Windows when PicoKey App is running. Enable
    PicoKey Agent for background monitoring when the main app is closed.

---

## Agent

### Enable Agent

Enables communication with **PicoKey Agent**.

When enabled:

- The device can communicate with the local PicoKey Agent
- Advanced operations become available
- Required for features such as:

  - Device time synchronization

  - Audit or traceability features

  - Notification handling like user presence, credential creation or credential assertion.

!!! note
    PicoKey Agent runs as a background service and must be installed separately.

---

## Device status indicators

At the bottom of the application, PicoKey App displays real-time status information:

- Device connection status (online/offline)
- Recovery mode state
- Current device time

!!! tip
    Device time is updated automatically when the agent is enabled.
