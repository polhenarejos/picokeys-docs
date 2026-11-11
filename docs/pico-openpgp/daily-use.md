# Daily use

Once the card is provisioned, you can use it for the usual OpenPGP workflows:

- signing
- decryption
- authentication

These workflows should behave predictably. If they do not, check the host setup and the credential role in use.

## Signing

The signature key is used for normal OpenPGP signing operations through GnuPG or another card-aware client.

Test a signing flow early with the client you plan to use. This confirms that:

- the card is visible
- `PW1` works
- the host really uses the card-backed key

## Decryption

The decryption path is where performance and middleware issues become more visible, especially with larger RSA keys.

Before treating a platform as supported, test a complete decryption workflow as well as card detection.

## Authentication

Authentication workflows depend on the host agent as well as the card. If you use the card for SSH or another agent-backed path, validate that workflow separately.

## UIF and confirmation

Upstream claims support for user-interaction flag behavior. If you enable confirmation requirements, document them clearly so operators understand why a command appears to wait on the device.
