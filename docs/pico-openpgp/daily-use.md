# Daily use

Once the card is provisioned, the day-to-day workflows are the ordinary OpenPGP ones:

- signing
- decryption
- authentication

The card should feel boring here. If it does not, the host setup or role model is probably still wrong.

## Signing

The signature key is used for normal OpenPGP signing operations through GnuPG or another card-aware client.

Verify one signing flow early with the exact client you intend to use later. This is the fastest way to confirm:

- the card is visible
- `PW1` works
- the host really uses the card-backed key

## Decryption

The decryption path is where performance and middleware issues become more visible, especially with larger RSA keys.

Test one full decrypt workflow, not just card enumeration, before calling a platform "supported."

## Authentication

Authentication workflows often depend on host-agent setup as much as on the card itself. If you use the card for SSH or similar agent-backed paths, treat that as its own validation item.

## UIF and confirmation

Upstream claims support for user-interaction flag behavior. If you enable confirmation requirements, document them clearly so operators understand why a command appears to wait on the device.
