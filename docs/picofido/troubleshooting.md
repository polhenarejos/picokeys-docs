# Troubleshooting

## The browser does not see the device

First check the USB connection and confirm the board booted into firmware:

- replug the board
- confirm the board really booted into firmware rather than USB mass-storage mode
- test with a second browser or a CLI FIDO tool

If neither the browser nor a CLI tool sees a FIDO device, resolve detection before debugging passkey registration.

## Registration prompts for a PIN unexpectedly

This prompt is often expected because the authenticator requires user verification or management authorization for the requested operation.

## A Yubico-oriented tool cannot see the device

The tool may be relying on a YubiKey-like USB identity or reader name. Check those requirements before treating the issue as a FIDO protocol failure.

## A relying party rejects an algorithm or extension

Do not assume firmware failure first. Check:

1. what the authenticator advertises
2. what the browser requests
3. what the relying party accepts

The authenticator, browser, and relying party each apply their own support rules.

## Resident credential management is inconsistent between clients

Clients expose resident-credential management differently. Test listing and deletion with the exact management path you plan to support.

## Security features behave differently across boards

If a claim about secure lock, secure boot, or OTP-backed protection seems inconsistent, confirm the exact board first. The supported hardware families do not share one identical security baseline.
