# Release notes

This page tracks notable PicoKey App features reflected in this documentation set.

---

## v2.4 baseline

Reference tag in `picokeyapp`: `v2.4` (`2026-02-16`).

---

## New in docs after v2.4

### FIDO

- Added **FIDO Dashboard** page with capability fields:
  - `AAGUID`
  - `Max credBlob`
  - `Large blob entries`
  - `Large blob usage`
  - options and extensions (active options highlighted in green)
- Expanded **Passkeys**:
  - copy credential ID
  - export passkey record (`JSON` / `CSV`)
  - delete passkey
  - manage credential `Large Blob` (when supported)
  - filters (`RP`, user, credential ID, etc.)
  - delete all passkeys for selected RP
  - credential metadata (`created`, curve, `credProtect`, signature counter state)
- Added vendor section **Vendor Features > User PIN**:
  - admin/user PIN contexts
  - permission matrix
  - default context behavior on reconnect
  - context switching flow and constraints
- Expanded **Accounts**:
  - OATH access control (`Set Access`, `Unlock`, `Change Access`, `Clear Access`)
  - rename/delete account operations
  - TOTP/HOTP code chip behavior and clipboard copy flow

### Audit

- Added **Audit** page:
  - audit purpose and event traceability
  - Audit PIN protection (`Set PIN` / `Change PIN`)
  - actions (`Refresh`, `Export Log`, `Clear Log`, `Reset`)
  - event flags (`Log truncated`, `Log cleared`, `Firmware changed`, `Log protected`, `Time unsynced`, `Write errors`)
  - total operations counter

### OpenPGP / PIV

- Expanded **OpenPGP Management**:
  - `PW1` vs `PW3` access model
  - admin-operation requirements
  - registration requirement note
- Expanded **PIV**:
  - unlock model (`Unlock` / `Lock`) with MGM key
  - rotate flow including `Delete permanently`
  - registration requirement note

### HSM

- Expanded **HSM Key management**:
  - blocked PIN recovery actions (`Unblock PIN`, `Reset Retries`)
  - private data object import actions
  - certificate delete action
  - registration requirement note

### Core app sections

- Expanded **Firmware**:
  - explicit local firmware load flow via **Search**
  - update trigger via **Upgrade**
  - `Wipe entire flash` option
  - registration/rescue behavior notes
- Expanded **Security**:
  - explicit apply action (`Enable Security Options`)
  - registration requirement note
- Expanded **Configuration**:
  - registration requirement note for commissioning
- Expanded **Home**:
  - memory info now documents `Firmware size`

---

## Notes

- This page summarizes documentation coverage updates, not full firmware/API changelogs.
- For implementation-level history, use `git log` in the `picokeyapp` repository.

## v3.0

PicoKey App 3.0 includes:

- a dedicated **FIDO > Vault** panel for local opaque credential packages;
- export and import of selected resident credentials through an enrolled Pico
  Vault;
- four Vault encryption profile choices;
- Vault metadata such as RP, user, algorithm, board serial, and Vault ID;
- Vault groups packages from multiple Vault IDs and permits import only when the
  board's active Vault ID matches;
- bulk Vault import/export and per-credential Vault actions;
- per-credential expiration and revocation actions;
- PIN complexity policy configuration;
- `alwaysUv`, `makeCredUvNotRqd`, and discoverable-credential controls;
- a FIDO Dashboard switch for the Keyboard Interface used by OTP slots 1–4;
- better FIDO transport fallback when the HID path is unavailable; and
- improved OpenPGP/PIV panel behavior for mixed FIDO2 devices.

The standalone [Pico Vault Enroller](https://github.com/polhenarejos/pico-vault-enroller)
is required for board enrollment. PicoKey App manages exported packages after
enrollment; it does not replace the enrollment ceremony.
