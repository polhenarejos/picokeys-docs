# Backup and restore

This document describes how to backup and restore keys in Pico HSM using wrapped keys.
The procedure is based directly on the official Pico HSM documentation.

Backup and restore operations allow exporting keys encrypted under a Device Key Encryption Key (DKEK), ensuring that private keys are never exposed in plaintext.

## Overview

The backup and restore mechanism relies on:

- A Device Key Encryption Key (DKEK)
- Wrapped key export
- Controlled key import on compatible devices

!!! warning
    Backup data must be protected carefully. Anyone with access to the wrapped keys and the DKEK may restore them.

## Generate DKEK shares

The DKEK is split into multiple shares using an n-of-m scheme.

Generate the DKEK shares:

```bash
pkcs11-tool \
  --generate-key \
  --key-type aes:32 \
  --label dkek \
  --id 01 \
  --pin 648219
```

Export the DKEK shares:

```bash
pkcs11-tool \
  --export-key \
  --id 01 \
  --pin 648219 \
  --output-file dkek-share.bin
```

!!! note
    The DKEK itself never leaves the device unencrypted.

## Backup (export) keys

To backup a private or secret key, export it wrapped under the DKEK.

```bash
pkcs11-tool \
  --export-key \
  --id 12 \
  --wrap \
  --wrapping-key 01 \
  --pin 648219 \
  --output-file key-backup.bin
```

This produces an encrypted backup file.

!!! tip
    Store backup files offline and protect them with the same care as the original device.

## Restore (import) keys

To restore a previously backed-up key:

```bash
pkcs11-tool \
  --import-key \
  --unwrap \
  --wrapping-key 01 \
  --pin 648219 \
  --input-file key-backup.bin \
  --id 12 \
  --label restored-key
```

If the DKEK matches, the key will be restored securely.

## Key compatibility

Backup files can only be restored:

- On devices with compatible firmware
- Using the same DKEK
- With matching cryptographic capabilities

!!! warning
    Restoring keys on incompatible devices may fail or result in unusable keys.

## Security considerations

When using backup and restore:

- Limit access to DKEK shares
- Avoid storing backups online
- Test restore procedures before relying on backups

!!! danger
    Loss of both the device and the DKEK makes key recovery impossible.

---

## Summary

The backup and restore mechanism in Pico HSM provides:

- Secure encrypted key export
- Controlled key import
- Protection against plaintext key exposure

This enables safe key migration and disaster recovery when used correctly.
