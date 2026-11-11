# First steps

Pico HSM is a smart-card-attached cryptographic device, so first confirm that the host can communicate with it.

## 1. Get the device detected

Upstream expects CCID and OpenSC-style tooling. Start with detection:

```sh
opensc-tool -l
opensc-tool -a
opensc-tool -an
```

These checks confirm that:

- the operating system sees a reader
- the middleware sees a card
- the device identity is routed through the expected driver path

If you cannot do that, do not jump straight to PKCS#11 commands.

## 2. Confirm PKCS#11 access

Once the reader path is healthy, list stored objects:

```sh
pkcs11-tool --list-objects --pin 648219
```

Replace the example PIN with your device's PIN. The object list is only a check that authenticated PKCS#11 traffic works end to end.

## 3. Start with a disposable key

Before importing real material or enabling advanced policies, generate a test key and prove you can:

- list it
- export the public part
- use it for one cryptographic operation

That sequence detects most middleware and session mistakes early.

## 4. Keep platform class in mind

The upstream security story is materially different across hardware:

- RP2040 is the weakest platform for at-rest protection
- RP2350/RP2354 and ESP32-S3 are where secure boot, secure lock, and OTP-backed claims become relevant

Record the exact board model whenever you evaluate the HSM, since platform choice affects its security properties.

## 5. Do not start with advanced workflows

Avoid this order:

- DKEK shares first
- wrapped-key import first
- secure messaging first

Use this order instead:

1. detect device
2. authenticate
3. generate test key
4. sign or decrypt test data
5. then move to wrapped-key and admin workflows

This sequence adds a few checks up front and makes later troubleshooting easier.
