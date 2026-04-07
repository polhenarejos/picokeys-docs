# User PIN (Vendor Extension)

This page documents Pico FIDO vendor extensions exposed in PicoKey App.

These behaviors are not part of baseline FIDO interoperability requirements and should be treated as PicoKey-specific management features.

---

## User PIN dual-context model

Pico FIDO can operate with two PIN contexts:

- **Admin PIN**: full management capabilities.
- **User PIN**: restricted capabilities defined by Admin.

Each context has its own PIN value.

When User PIN mode is enabled:

- Default context on reconnect is **User**.
- Admin can switch context and manage User permissions.

When User PIN mode is disabled:

- Only Admin context is active.

---

## User PIN permission set

The User context permission mask supports:

- Create credentials
- Credential management
- Authentication
- Large Blob write
- Configure authenticator
- Persistent credential management
- Reset authenticator

Default behavior:

- All User permissions start enabled.
- Authentication remains enabled to preserve normal login/authentication usability.

---

## Extended passkey management

PicoKey App exposes extended passkey operations on top of standard credential management:

- Copy credential ID
- Export passkey metadata record (`JSON` / `CSV`)
- Delete per credential
- Delete all credentials for selected RP
- Search/filter by RP, user, credential ID, and related fields

Displayed passkey metadata can include:

- Creation date
- Algorithm/curve details
- `credProtect` state
- Signature counter state

---

## Per-credential Large Blob handling

For credentials with `largeBlobKey` support, PicoKey App provides per-credential Large Blob management (read/update style workflow).

This is implemented as an application management workflow over authenticator large blob storage.

---

## Dashboard capability view

PicoKey App FIDO dashboard surfaces capability/status fields useful for vendor management workflows:

- `AAGUID`
- Max `credBlob`
- Large blob entries count
- Large blob usage
- Reported options (active options highlighted)
- Reported extensions

Use these values to validate effective runtime capability before applying vendor policy/configuration decisions.
