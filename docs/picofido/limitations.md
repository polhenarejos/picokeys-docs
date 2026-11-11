# Limitations

These are the limits most likely to affect a deployment.

## Hardware security is not uniform

- RP2040 does not provide the same at-rest story as RP2350/RP2354 or ESP32-S3 class hardware.
- Secure Boot, Secure Lock, and OTP-backed secrets only matter on hardware that actually implements them.
- Pico FIDO is still a microcontroller-based authenticator, not a secure element.

## Host compatibility is uneven

- standard WebAuthn browser flows are the primary target
- Yubico-oriented tooling may depend on reader identity heuristics
- vendor extensions are less portable than baseline CTAP features

If a feature works only in one management app, document that explicitly.

## Extension support is not relying-party support

The firmware may advertise:

- `credProtect`
- `credBlob`
- `hmac-secret`
- large blobs
- `minPinLength`

The relying party, browser, and operating system must also support an extension before it affects a login flow.

## Legacy OTP features are compatibility features

OATH, Yubico OTP, and static-password slots are useful. They are not more modern or more phishing-resistant than passkeys.

Use them to support older systems, while keeping passkeys as the primary choice when the service supports them.

## Recovery needs testing

Test a reset, reprovisioning, and re-enrollment before relying on the device's recovery process.

## Pico Vault is deliberately limited

Pico Vault is not a general private-key export mechanism and is not part of
ordinary WebAuthn interoperability.

- Enrollment requires the separate Vault enroller, a license, PIN
  authorization, and physical presence.
- Import requires the destination board to belong to the same Vault domain.
- The local enrollment envelope and its passphrase are both required for
  replacement-board recovery.
- A credential must be exported while the source board and Vault material are
  available; Vault does not reconstruct credentials after both are lost.
