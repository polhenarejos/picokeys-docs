# Passkeys

This page describes the **Passkeys panel** in PicoKey App FIDO management.

![Passkeys view](../../assets/images/picokeyapp/fido/passkeys.png)

---

## Overview

The Passkeys panel lists resident FIDO credentials stored on the device and their metadata.

- Credential source and relying party information
- Username / user handle when available
- Per-credential management actions

---

## Per-credential actions

Each credential row can expose these actions:

- **Copy Credential ID**
- **Export passkey record** (`JSON` or `CSV`)
- **Delete**

The export action stores passkey metadata (for example RP, user fields, and credential ID) for administration and backup workflows.

For credentials that support it, the panel also provides a **Large Blob** management action.

![Large Blob management](../../assets/images/picokeyapp/fido/largeblobs.png)

---

## Credential details shown

The Passkeys list includes additional metadata to help administration and troubleshooting:

- Creation date
- Curve type / key algorithm
- `credProtect` policy in use
- Signature counter status (enabled/active or not)

---

## Filtering and bulk operations

The panel supports filtering credentials by common fields, including:

- RP (Relying Party)
- User
- Credential ID
- Other searchable credential fields shown in the table

Administrative operations also include deleting all credentials for a specific RP.

---

## Locked state

When credentials are protected by PIN or policy, the panel requires user verification before showing sensitive data.

![Passkeys unlock](../../assets/images/picokeyapp/fido/passkeys-unlock.png)

!!! note
    Exact fields and actions depend on the installed firmware.
