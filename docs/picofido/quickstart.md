# Quick start

This quick start takes a Pico FIDO device from flashing through passkey registration and one recovery check.

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

Start with a standard WebAuthn site; if the browser detects the device, the FIDO connection is working.

An alternative CLI-level check is a libfido2-based tool such as:

```sh
fido2-token -L
```

If the device is not visible here, stop and fix detection first.

## 3. Set a PIN

Set a PIN before enrolling real credentials. The tool depends on your host setup, but you should confirm that:

- a PIN exists
- the device prompts for it when a UV-gated operation requires it
- retry behavior is understood before the device holds anything important

## 4. Register one passkey

Use a standard WebAuthn registration flow and verify:

- registration succeeds
- physical confirmation works
- login with the same credential succeeds


## 5. Test one management action

Before calling the setup done, also test one stored-credential management action:

- list a resident credential
- or delete one test credential and confirm the relying party behavior

This confirms that both authentication and the management path you plan to use are working.

## 6. Test one destructive recovery path on non-critical hardware

Before trusting a deployment model, test at least one reset flow on a disposable setup:

- set PIN
- enroll a credential
- reset
- confirm what survived and what did not

This test gives you evidence that recovery works on the setup you plan to use.
