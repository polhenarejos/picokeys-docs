# Passkeys

This page explains Pico FIDO passkey capabilities in detail: protocols, operations, extensions, algorithms, and security controls.

---

## Protocol stack

Pico FIDO passkey flows are built on:

- CTAP1/U2F (`U2F_V2`)
- CTAP2 (`FIDO_2_0`, `FIDO_2_1`, `FIDO_2_2`)
- WebAuthn in browsers/OS clients over HID transport

!!! note
    The effective behavior is what the device reports in `authenticatorGetInfo` at runtime.

---

## Core passkey operations

Pico FIDO supports the main passkey lifecycle:

- `makeCredential` (registration)
- `getAssertion` (authentication)
- Discoverable/resident credentials (`rk`)
- Credential management (enumeration and delete)
- Authenticator configuration (`authnrCfg`)

---

## Supported algorithms and curves

For passkeys, Pico FIDO advertises COSE algorithms through `getInfo.algorithms`. Typical builds include:

- ES256 (`secp256r1`)
- ES384 (`secp384r1`)
- ES512 (`secp521r1`)
- ES256K (`secp256k1`) when enabled
- EdDSA (`Ed25519`) when enabled

---

## Extensions support

Pico FIDO `getInfo.extensions` includes support for:

- `credBlob`
- `credProtect`
- `hmac-secret`
- `hmac-secret-mc`
- `largeBlobKey`
- `minPinLength`
- `thirdPartyPayment`

Additional related capabilities/options include:

- Large blob storage (`largeBlobs`, with serialized large blob array size limit)
- Enterprise attestation option (`ep`) when enabled
- Self attestation path (firmware/config dependent)

---

## PIN/UV and policy controls

Security controls exposed by CTAP options include:

- `clientPin`
- `pinUvAuthToken`
- `setMinPINLength`
- `alwaysUv` (state-dependent)
- User presence checks (`UP`) for protected operations

PIN/UV protocols include:

- PIN/UV protocol 1
- PIN/UV protocol 2

`minPinLength` policy can be configured per RP ID set (authenticator configuration flow).

---

## Credential management details

Credential management supports:

- Metadata query (existing credentials and remaining capacity)
- RP enumeration
- Credential enumeration per RP
- Credential deletion

These operations require proper permissions and valid PIN auth token.

---

## Attestation and interoperability

Passkey registration supports attestation flows including:

- Standard packed attestation
- Self attestation (when configured)
- Enterprise attestation modes (when configured and allowed)

Compatibility focus:

- Browser/WebAuthn clients
- Yubico tooling and ecosystem-compatible apps in supported feature areas

---

## Important practical note

Feature availability can vary by:

- firmware version
- compile-time options (for example EdDSA / extra curves)
- hardware profile (RP2040 vs RP2350/ESP32-S3 security capabilities)

For operational decisions, use what your device currently reports via `authenticatorGetInfo`.
