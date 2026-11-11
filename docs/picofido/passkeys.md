# Passkeys and CTAP

Pico FIDO supports passkeys, second-factor security-key logins, discoverable credentials, and legacy U2F.

Pico FIDO speaks CTAP over HID, allowing standard WebAuthn browsers and operating-system dialogs to handle core FIDO flows without smart-card middleware. Host tools still vary in the details they expose.

## Registering and logging in

The core flow is:

- `makeCredential` registers a credential
- `getAssertion` uses it later
- discoverable credentials let the authenticator identify an account without an allow-list
- credential management lists and deletes stored records when policy permits

The protocol flow is consistent, while client interfaces differ in how they expose it:

- browsers expose different amounts of detail
- resident-credential cleanup is surfaced differently by each platform
- vendor-aware tools may show more than generic tools

## Touch and user presence

Upstream documents user presence through a physical button, so confirm that the expected button behavior is part of your device setup.

If you are testing on custom hardware:

- verify that the board really exposes the expected confirmation path
- do not assume every board profile gives the same physical interaction behavior

On supported boards with confirmation wiring, the device requires a user confirmation before completing the requested operation.

## Set a PIN first

Before relying on the device, set a PIN and verify that user verification behaves as expected.

Set the PIN early because:

- many management operations depend on it
- some relying parties request UV
- it is the boundary between casual presence-only use and authenticated user context

If a registration suddenly starts prompting for a PIN, that is often expected behavior, not a malfunction.

## Resident passkeys

Upstream advertises discoverable credentials and credential management, which support the passkey workflows most users rely on.

The important operational consequences are:

- credentials live on the device
- capacity is finite
- deletion and enumeration depend on the management path you use

If you are validating Pico FIDO for daily passkey use, test these explicitly:

1. register a resident credential
2. log in with it
3. list it with your intended management tool
4. delete it and verify the relying party behavior afterwards

Testing this flow confirms that the full lifecycle works with your chosen management tool.

## Non-resident registrations and U2F

Pico FIDO also supports classic second-factor style registrations and legacy U2F compatibility according to upstream.

These flows are often easier to operate because:

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

Host support varies here, so check each layer separately:

- firmware support is necessary
- client support is separate
- relying-party support is separate again

Test the exact combination you plan to depend on.

## Credential policy in Pico FIDO 8

Pico FIDO 8 adds policy controls that are visible through vendor management
tools such as PicoKey App:

- **Always UV** can require user verification for every applicable operation.
- **Make credential UV not required** can relax the user-verification flag for
  newly created credentials when policy allows it.
- **Resident credential policy** can disable discoverable credentials when a
  deployment allows only non-resident credentials.
- **PIN complexity** can require uppercase letters, lowercase letters, digits,
  symbols, or selected combinations.
- **Credential expiration** causes a credential to be rejected after its
  configured date.
- **Credential revocation** permanently prevents further assertions with the
  revoked credential.

These are device policy controls, not WebAuthn relying-party settings. Record
them as part of device commissioning because they can change registration and
login behavior.

## Attestation

Upstream also claims:

- self attestation
- enterprise attestation

Treat attestation as a deployment choice because it affects trust and privacy assumptions. Enable it only when your relying parties and operators need it.

## Reset and recovery

A proper FIDO evaluation also needs one destructive test on non-critical hardware:

- set a PIN
- enroll one resident credential and one classic second-factor credential
- perform a reset
- confirm what survived and what did not

If you have never tested reset behavior, your recovery story is probably incomplete.
