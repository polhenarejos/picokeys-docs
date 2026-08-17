# Pico FIDO 8 release notes

Pico FIDO 8 adds authenticated passkey portability, broader FIDO 2.3
behavior, stronger credential policy controls, and more resilient protected
storage.

## Pico Vault

- Export a selected resident credential as an authenticated opaque PKV1 package.
- Import it onto another board enrolled in the same Vault domain.
- Protect enrollment with PIN authorization, certificate-backed device
  identity, board binding, and a physical-button ceremony.
- Preserve credential metadata during export and keep imported credentials
  separate from native credentials.
- Choose ChaChaPoly, AES-GCM, or either two-layer combination.
- Use the standalone [Pico Vault Enroller](https://github.com/polhenarejos/pico-vault-enroller)
  to create the encrypted enrollment envelope and provision a board.

Vault is a gated vendor capability. It does not change ordinary WebAuthn or
CTAP credential behavior.

## FIDO and WebAuthn

- Announce and exercise FIDO 2.3 behavior.
- Expand `thirdPartyPayment`, `uvm`, `minPinLength`, and `maxPINLength` support.
- Configure PIN complexity rules for uppercase, lowercase, digits, symbols, or
  combinations.
- Configure `alwaysUv`, `makeCredUvNotRqd`, and resident/non-resident
  credential policies.
- Set per-credential expiration dates and revoke credentials explicitly.
- Improve credential management, RP binding, large credential sets, and
  large-blob validation.
- Improve HID cancellation, selection, and next-assertion handling.

## Storage and recovery

- Resident credentials, OATH accounts, and OTP slots use authenticated object
  containers while retaining compatibility with supported legacy layouts.
- Protected updates use stronger validation and crash-safe publication.
- PIN retry state and authorization state survive failure and power-loss cases
  more reliably.
- Sensitive configuration, backup, password-safe, OATH, OTP, and Vault
  operations enforce the required authorization and physical presence.

## Testing and upgrade note

The release includes expanded FIDO 2.3, credential-management, OATH, OTP,
storage-compatibility, and Vault tests. An internal conformance run recorded
251 passes and 0 failures; this is test evidence, not FIDO Alliance
certification.

Pico FIDO 8 changes internal credential storage. Keep a tested recovery path
before upgrading a device with important credentials. Vault recovery is only
available for credentials exported while the source device and its Vault
material are still available.
