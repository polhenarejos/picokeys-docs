# OTP slots

Pico FIDO also exposes Yubico-style OTP slot behavior. Upstream documents four slots, each holding one configured function.

## Slot model

Each slot can be configured independently as one of:

- static password
- HOTP
- Yubico OTP
- challenge-response

That flexibility is useful, but it also means the USB keyboard path can easily outlive the use case that justified it.

## Activation

The current docs describe BOOTSEL-based activation by repeated button presses:

- slot 1: one press
- slot 2: two presses
- slot 3: three presses
- slot 4: four presses

Treat that as a physical UI contract. If you deploy it operationally, make sure users understand which slot does what before they start typing secrets into the wrong input field.

## Output behavior

For static password, HOTP, and Yubico OTP modes, the device can emit characters through the USB keyboard interface.

Challenge-response is different:

- it is not meant to "type" a value
- it computes a response from a host-supplied challenge

That difference is easy to gloss over and often confuses users.

## When these slots are a good fit

They make sense when you need compatibility with:

- older systems that still expect OTP
- workflows already built around Yubico-style slot semantics
- challenge-response integrations that are not WebAuthn

They are not a better replacement for passkeys. They are a fallback for older ecosystems.

## Security cautions

- static passwords are only as safe as the place they are typed
- OTP keyboard emission is vulnerable to input focus mistakes
- physical possession may be enough to trigger output if extra controls are not enabled

If the goal is phishing-resistant authentication, use passkeys and keep slot features only where legacy compatibility forces the issue.
