# Initialization and policy

This page describes Pico FIDO firmware initialization and policy controls.

---

## Firmware controls

Pico FIDO firmware supports initialization and policy settings such as:

- PIN setup and user-verification enforcement
- Attestation behavior
- Minimum PIN length policy
- RP ID restrictions for policy-controlled operations

These parameters define security posture and authenticator behavior before normal passkey/account usage.

---

## Policy parameters

Key policy parameters include:

- `PIN`: enables user verification and protects sensitive authenticator operations.
- `Attestation`: controls whether attestation data is emitted in registration flows.
- `Minimum PIN length`: enforces lower bound for future PIN values.
- `RP ID restrictions`: limits policy scope to selected relying parties.

---

## Operational notes

- Initialization choices affect passkeys, credential visibility, and management operations.
- Restrictive policy settings can block expected authentications if RP IDs are misconfigured.
- Some changes may require reset/reprovisioning depending on current device state and policy.
