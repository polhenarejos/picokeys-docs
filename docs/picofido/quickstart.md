# Quick start

From zero to a working Pico FIDO device in a short test cycle: flash it, set a PIN, register one passkey, and verify one recovery path.

## What you need

- a supported board
- a USB cable
- a firmware image, either prebuilt or built from source
- one host with a modern browser

If you plan to validate OATH or OTP behavior as well, keep the relevant host tool ready, but do not start there.

## 1. Flash the firmware

For Raspberry Pi Pico-family boards:

1. hold `BOOTSEL`
2. connect the board
3. copy `pico_fido.uf2`
4. let the board reboot

For ESP32-S3 boards, use the upstream flasher flow.

## 2. Confirm the host sees a FIDO authenticator

The simplest check is a standard WebAuthn site. If the browser sees the device, the FIDO path is alive.

An alternative CLI-level check is a libfido2-based tool such as:

```sh
fido2-token -L
```

If the device is not visible here, stop and fix detection first.

## 3. Set a PIN

Set a PIN before real enrollment. The exact tool depends on the host path you use, but the outcome should be the same:

- a PIN exists
- the device prompts for it when a UV-gated operation requires it
- retry behavior is understood before the device holds anything important

## 4. Register one passkey

Use a standard WebAuthn registration flow and verify:

- registration succeeds
- physical confirmation works
- login with the same credential succeeds

This proves the core FIDO path, not just enumeration.

## 5. Test one management action

Before calling the setup done, also test one stored-credential management action:

- list a resident credential
- or delete one test credential and confirm the relying party behavior

The point is to verify not only authentication but also the management path you expect to use later.

## 6. Test one destructive recovery path on non-critical hardware

Before trusting a deployment model, test at least one reset flow on a disposable setup:

- set PIN
- enroll a credential
- reset
- confirm what survived and what did not

This is the difference between "we assume recovery works" and "we have seen it work."
