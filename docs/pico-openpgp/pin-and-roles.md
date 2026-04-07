# PIN and roles

This page summarizes PIN domains and operational roles for Pico OpenPGP workflows.

---

## PIN domains

OpenPGP card workflows commonly use:

- `PW1`: user operations context
- `PW3`: admin operations context
- `RC`: reset/recovery code domain

Retry counters and lock behavior are enforced by card policy and firmware state.

---

## What each domain is typically used for

### `PW1` (user)

Commonly used for user-facing operations such as:

- signing/authentication flows
- regular use of already-provisioned key material
- day-to-day card usage in client applications

### `PW3` (admin)

Typically required for administrative operations such as:

- sensitive configuration changes
- key-attribute or policy-level changes
- destructive/recovery-sensitive actions

### `RC` (reset code)

Recovery-oriented domain used for controlled reset/recovery paths when applicable.

---

## Operational role split

In practice:

- User-context tasks are performed through `PW1`
- Administrative/security-sensitive tasks require `PW3`

This split is important for:

- key-management changes
- policy modifications
- reset/recovery actions

In team environments, this usually maps naturally to:

- **Operator/user role**: daily cryptographic use
- **Security admin role**: lifecycle, policy, and recovery controls

---

## Session and unlock expectations

Role enforcement is not only about stored PIN values, but also current session state.

Practical implications:

- A panel can be visible but still not writable until proper unlock context is active.
- Unlocking with user context does not automatically grant admin actions.
- Reconnects/session resets can require re-authentication for protected operations.

---

## Retry and lock handling

PIN retry counters are part of operational safety:

- Wrong PIN attempts reduce retries.
- Reaching limit can block the corresponding credential path.
- Recovery behavior depends on enabled card policy and available recovery credentials.

For production use, define clear operational procedures for:

- PIN rotation cadence
- secure storage of admin/recovery credentials
- lockout/recovery runbooks

---

## PicoKey App alignment

For UI-level role and PIN workflows, see:

- [OpenPGP Management](../picokeyapp/openpgp/management.md)
- [PIV](../picokeyapp/openpgp/piv.md)

Recommended approach:

1. Validate current unlock context.
2. Perform read-only checks first.
3. Execute admin changes only under confirmed admin context.
4. Re-verify expected state after each sensitive change.
