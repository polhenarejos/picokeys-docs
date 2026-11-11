# Host setup and compatibility

When OpenPGP does not work, check the host stack before changing card settings.

Pico OpenPGP is a CCID smart-card device, so the minimum host stack is not optional:

- PC/SC must be running
- the reader must be recognized
- the middleware must know how to talk to the card
- the client tool must expose the workflow you want

## Check the card is visible

Start with a read-only status check:

```sh
gpg --card-status
```

If the card is not visible, fix detection before trying key generation or changing card policy.

Upstream also explicitly points to:

- OpenSC
- PKCS#11-capable applications
- `pkcs15-tool`

So a sensible validation order is:

1. confirm enumeration at the operating-system level
2. confirm PC/SC is running
3. confirm `gpg --card-status` or an equivalent read-only command works
4. only then test a PIN-gated operation

## VID/PID and middleware recognition

The upstream README is explicit about a recurring CCID problem: host middleware may need the device identity to be recognized correctly.

Depending on the setup, you may need to:

- using PicoKey App to help commission the board
- building with a chosen VID/PID
- adjusting local driver or middleware configuration

Reader and driver configuration can be inconvenient, but it is a normal part of smart-card setup.

!!! warning
    "The board flashed successfully" and "the host stack will recognize the device properly" are different conditions.

## What usually works

When the host is healthy, the straightforward paths are usually:

- card detection
- metadata and status reads
- mainstream signing and decryption flows through GnuPG or OpenSC-backed clients

## What is more uneven

The less predictable paths are:

- advanced card management
- specialized APDU sequences
- AES-related functionality
- client behaviors that depend on a tool exposing a newer OpenPGP card feature cleanly

## Recommended compatibility checklist

For every platform you want to call supported, verify at least:

1. card detection
2. `gpg --card-status`
3. one `PW1`-gated action
4. one `PW3`-gated action
5. one signing operation
6. one decryption operation if you need it

Repeat those checks whenever you validate a platform; a one-off success does not establish reliable support.
