# First steps

Treat Pico HSM like a smart-card-attached cryptographic device, not like a USB file you can start using blindly.

## 1. Get the device detected

Upstream expects CCID and OpenSC-style tooling. Start with detection:

```sh
opensc-tool -l
opensc-tool -a
opensc-tool -an
```

You want to prove three things:

- the operating system sees a reader
- the middleware sees a card
- the device identity is routed through the expected driver path

If you cannot do that, do not jump straight to PKCS#11 commands.

## 2. Confirm PKCS#11 access

Once the reader path is healthy, list stored objects:

```sh
pkcs11-tool --list-objects --pin 648219
```

Use the real PIN for your device, obviously. The purpose here is not the object list itself. It is to confirm that authenticated PKCS#11 traffic works end to end.

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

If you are evaluating the HSM seriously, note the exact board used during testing.

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

It is slower once, and faster overall.
