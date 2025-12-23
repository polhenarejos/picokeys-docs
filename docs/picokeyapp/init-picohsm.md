# Pico HSM Initialization

This page documents the **security initialization options** available for **Pico HSM** when using PicoKeyApp.

These settings control **PIN policies, key protection mechanisms, and recovery options**.

![Pico HSM initialization](../assets/images/picokeyapp/init-picohsm.png)

---

## PIN configuration

### User PIN

Defines the primary user PIN.

- Required for cryptographic operations
- Protects private keys and stored objects

!!! tip
    The default PIN is `648219`.

---

### SO PIN

Defines the Security Officer PIN.

- Used for administrative operations
- Required for certain reset and recovery commands

!!! note
    The SO PIN should be stored separately from the user PIN.

---

### PIN retries

Configures the number of allowed incorrect PIN attempts.

- Applies to both user PIN and SO PIN
- Exceeding the limit blocks access

!!! danger
    Exceeding retry limits may permanently lock the device.

---

## Advanced protection options

### DKEK shares

Enables **Distributed Key Encryption Key** (DKEK) shares.

- Splits the encryption key into multiple shares
- Requires a threshold number of shares for recovery

!!! note
    DKEK improves resilience against single-point compromise.

---

### PUK authentications

Enables PUK-based recovery.

- Allows PIN reset using the PUK
- Adds an additional recovery mechanism

---

### PUK minimum authentications

Defines the minimum number of PUK authentications required.

- Used when PUK-based recovery is enabled

---

### Key domains

Enables key domain separation.

- Keys are grouped into isolated domains
- Access policies can differ per domain

---

## Additional options

### Reset retry counter command

Allows resetting the retry counter via command.

- Useful for controlled recovery scenarios

---

### Transport PIN

Allows setting a temporary PIN during transport or provisioning.

!!! warning
    Transport PINs should be changed immediately after deployment.

---

### PKA replaceable

Allows replacement of public key authentication data.

---

### Combined PIN & PKA authentication

Requires both PIN and public key authentication.

- Provides multi-factor protection

---

## Initialization process

After configuring all parameters:

- Click **Initialize**
- The device is provisioned with the selected security configuration

!!! danger
    Some options cannot be changed without reinitializing the device.

---

## Summary

Pico HSM security initialization defines:

- Access control policies
- Recovery mechanisms
- Cryptographic isolation guarantees

These options should be configured carefully according to the intended deployment model.
