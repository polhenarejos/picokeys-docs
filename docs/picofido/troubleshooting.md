# Troubleshooting

## The browser does not see the device

Start with the obvious checks:

- replug the board
- confirm the board really booted into firmware rather than USB mass-storage mode
- test with a second browser or a CLI FIDO tool

If the browser cannot see any FIDO device at all, do not debug passkeys yet.

## Registration prompts for a PIN unexpectedly

That is often expected. A PIN-gated flow is not a failure; it is the authenticator enforcing user verification or management authorization.

## A Yubico-oriented tool cannot see the device

This may be an identity or reader-name issue rather than a FIDO protocol issue. Verify whether the tool depends on a YubiKey-like USB identity or reader naming convention.

## A relying party rejects an algorithm or extension

Do not assume firmware failure first. Check:

1. what the authenticator advertises
2. what the browser requests
3. what the relying party accepts

These are three different layers.

## Resident credential management is inconsistent between clients

That is common. Test resident-credential listing and deletion with the exact management path you plan to support operationally.

## Security features behave differently across boards

If a claim about secure lock, secure boot, or OTP-backed protection seems inconsistent, confirm the exact board first. The supported hardware families do not share one identical security baseline.
