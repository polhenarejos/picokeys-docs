# Security initialization

This page describes the **security initialization and provisioning options** available with PicoKey App.

These options are **applied once during initialization** and directly affect the security model of the authenticator.

![Home](../assets/images/picokeyapp/security.png)

## Secure boot and lock options

### Secure Boot

When enabled:

- Firmware authenticity is verified at boot
- Only signed firmware images are allowed

!!! note
    Secure Boot is strongly recommended for production devices.

---

### Secure Lock

Locks the boot keys permanently.

- Prevents boot key replacement
- Protects against firmware downgrade attacks

!!! danger
    Secure Lock is irreversible. Enabling it incorrectly may permanently brick the device.

---

## Applying security options

After selecting the desired settings, click **Enable Security Options** to apply them on device.

---

## Registration requirement

This panel requires a registered board in PicoKey App before security actions can be applied.
