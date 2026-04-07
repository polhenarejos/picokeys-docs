# Compatibility notes

This page summarizes practical interoperability expectations for Pico OpenPGP.

---

## Common host tooling

Pico OpenPGP is designed to interoperate with standard smart-card ecosystems:

- GnuPG smart-card flows
- OpenSC stack and related utilities
- PKCS#11-capable applications
- PKCS#15 tooling

Typical examples:

- `gpg --card-status`, `gpg --edit-card --expert`
- `pkcs11-tool` operations through OpenSC
- `pkcs15-tool -D` for card object listing

---

## Compatibility model (practical)

For day-to-day OpenPGP smart-card usage, compatibility is usually strong when the host stack is correctly configured.

Workflows that are generally straightforward:

- Card detection and metadata reads
- OpenPGP signing/decryption workflows through standard smart-card stacks
- Basic key/certificate management exposed by common OpenPGP tooling

Workflows that may require extra care:

- Advanced card features not fully surfaced by every client UI
- Operations depending on specific driver/module behavior
- Vendor-specific or less-common command paths

---

## Standard vs specialized operations

Typical OpenPGP smart-card operations (key generation, signatures, decrypt workflows, card metadata) are expected to work through standard tooling when host configuration is correct.

Some advanced flows may require specialized tooling or direct APDU-level integration depending on client support.

Examples where specialized handling is common:

- Features available in firmware but not exposed by a specific GUI client
- Card operations requiring exact APDU sequencing
- Environment-specific PKCS#11 integration differences

---

## Host-side prerequisites

Interoperability depends on host configuration:

- Working PC/SC service
- CCID stack availability
- Reader/device VID/PID recognition in host middleware when required

If detection fails, verify PC/SC state, reader configuration, and middleware device lists first.

---

## Fast validation checklist

Use this short checklist before deeper troubleshooting:

1. Confirm the device is visible in PicoKey App.
2. Confirm PC/SC service is running on host.
3. Check smart-card tool can see the device (OpenSC/GnuPG path).
4. Validate one read-only operation first (status/info).
5. Then validate one protected operation (requires PIN/session).

If read-only works but protected operations fail, investigate PIN/session/auth state before driver-level debugging.

---

## Typical mismatch sources

Most reported compatibility issues come from:

- host middleware configuration
- conflicting smart-card services or stale readers
- unsupported or unlisted VID/PID in local driver configs
- stale card/session state after reconnects

Treat these as environment issues first, firmware issues second.
