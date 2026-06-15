# Setup and policy

From zero to a working Pico FIDO device: flash it, confirm the host sees it, and set the policies that matter before you enroll real credentials.

## Build or download

Upstream supports two normal paths:

- download a prebuilt image for your board
- build from source with your own board and USB identity settings

For Raspberry Pi Pico-family boards, the upstream build sequence is:

```sh
git clone https://github.com/polhenarejos/pico-fido
git submodule update --init --recursive
cd pico-fido
mkdir build
cd build
PICO_SDK_PATH=/path/to/pico-sdk cmake .. -DPICO_BOARD=board_type -DUSB_VID=0x1234 -DUSB_PID=0x5678
make
```

If you omit `PICO_BOARD`, `USB_VID`, and `USB_PID`, upstream says the defaults are `pico` and `FEFF:FCFD`.

The upstream README also documents `VIDPID=value` presets such as `Yubikey5`, `NitroFIDO2`, and others. Those presets are useful for interop testing. They should not be confused with permission to redistribute firmware under someone else's USB identity.

## Flash the board

For UF2-capable Raspberry Pi Pico-family boards:

1. Hold `BOOTSEL` while connecting the board.
2. Copy `pico_fido.uf2` to the mass-storage device.
3. Let the board reboot.
4. Confirm the LED reaches its normal idle state.

For ESP32-S3 boards, upstream sends users through the ESP32 flasher flow instead.

## First validation

Pico FIDO uses the FIDO HID interface for WebAuthn and CTAP. That means basic FIDO use does not depend on CCID or smart-card middleware.

Check the minimum first:

- open a browser and try a WebAuthn registration flow
- or use a libfido2-based tool such as `fido2-token -L`
- confirm the device is visible before changing policy

If you are testing `ykman`, Yubico Authenticator, or Nitrokey-oriented tools, be careful: those tools may care about reader name, VID/PID, or both.

## Set a PIN early

Do this before real enrollment if the device will hold anything important.

The reasons are the same as on other FIDO authenticators:

- many management operations require PIN-authenticated context
- relying parties may request user verification
- a device with no PIN is easier to misuse locally

Upstream also claims support for `minPinLength`. That is useful, but it is only useful if your operators understand the policy and your clients behave correctly once it is enforced.

## Attestation and policy choices

Upstream advertises:

- self attestation
- enterprise attestation
- RP-scoped restrictions and related policy controls

Use these deliberately.

!!! note
    "The firmware can emit attestation" is not the same thing as "your relying parties should trust or require it."

Restrictive RP policy is similar: it is valuable only if you document it well enough that later failures are explainable.

## Hardware note

From a security point of view, board choice is not cosmetic:

- RP2040 is the weakest platform in this family for at-rest protection
- RP2350/RP2354 and ESP32-S3 are where upstream's Secure Boot, Secure Lock, and OTP-backed claims become relevant

If you are evaluating Pico FIDO seriously, always record which board you used for the result.
