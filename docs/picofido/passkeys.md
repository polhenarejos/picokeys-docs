# Passkeys and CTAP

The FIDO half of the device: passkeys, second-factor security-key logins, discoverable credentials, and legacy U2F.

Pico FIDO speaks CTAP over HID, so standard WebAuthn browsers and OS dialogs should drive the core FIDO flows without extra smart-card middleware. What varies in practice is not the protocol but the host tooling around it.

## Registering and logging in

The normal mental model is:

- `makeCredential` registers a credential
- `getAssertion` uses it later
- discoverable credentials let the authenticator identify an account without an allow-list
- credential management lists and deletes stored records when policy permits

That is the stable part. The unstable part is client UX:

- browsers expose different amounts of detail
- resident-credential cleanup is surfaced differently by each platform
- vendor-aware tools may show more than generic tools

## Touch and user presence

Upstream documents user presence through a physical button. That should be treated as a real part of the device contract, not as a decorative feature.

If you are testing on custom hardware:

- verify that the board really exposes the expected confirmation path
- do not assume every board profile gives the same physical interaction behavior

For supported boards with proper confirmation wiring, a FIDO operation is not just "the host asked"; it is "the host asked and the user confirmed."

## Set a PIN first

Before relying on the device, set a PIN and verify that user verification behaves as expected.

Why first:

- many management operations depend on it
- some relying parties request UV
- it is the boundary between casual presence-only use and authenticated user context

If a registration suddenly starts prompting for a PIN, that is often expected behavior, not a malfunction.

## Resident passkeys

Upstream advertises discoverable credentials and credential management. These are the modern passkey workflows that matter most in practice.

The important operational consequences are:

- credentials live on the device
- capacity is finite
- deletion and enumeration depend on the management path you use

If you are validating Pico FIDO for daily passkey use, test these explicitly:

1. register a resident credential
2. log in with it
3. list it with your intended management tool
4. delete it and verify the relying party behavior afterwards

That is more meaningful than reading a raw feature list.

## Non-resident registrations and U2F

Pico FIDO also supports classic second-factor style registrations and legacy U2F compatibility according to upstream.

These flows are usually easier operationally because:

- they do not rely on discoverable storage in the same way
- many mainstream services still understand them well

They are still worth testing if your deployment includes older services.

## Advertised algorithms

Upstream claims support for:

| COSE algorithm family | Curve / scheme |
| --- | --- |
| ES256 | `secp256r1` |
| ES384 | `secp384r1` |
| ES512 | `secp521r1` |
| ES256K | `secp256k1` |
| EdDSA | `Ed25519` |

The conservative interoperability default remains ES256. The fact that the firmware can advertise more does not mean every relying party will accept all of them.

## Extensions

Upstream advertises support for:

- `hmac-secret`
- `credProtect`
- `minPinLength`
- `credBlob`
- `largeBlobKey`
- large blobs

This is exactly the kind of area where host support is uneven. A good rule is:

- firmware support is necessary
- client support is separate
- relying-party support is separate again

Test the exact combination you plan to depend on.

## Attestation

Upstream also claims:

- self attestation
- enterprise attestation

Treat these as deployment features, not default recommendations. They affect trust and privacy assumptions and should not be enabled casually just because they are available.

## Reset and recovery

A proper FIDO evaluation also needs one destructive test on non-critical hardware:

- set a PIN
- enroll one resident credential and one classic second-factor credential
- perform a reset
- confirm what survived and what did not

If you have never tested reset behavior, your recovery story is probably incomplete.
