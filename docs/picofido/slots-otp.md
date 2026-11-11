# OTP slots

Pico FIDO also provides Yubico-style OTP slots. Upstream documents four slots, and each slot holds one configured function.

## Slot model

Each slot can be configured independently as one of:

- static password
- HOTP
- Yubico OTP
- challenge-response

This flexibility helps with older integrations, although the USB keyboard behavior may remain enabled after the original use case has changed.

## Activation

On supported firmware, activate a slot by pressing the BOOTSEL button repeatedly:

- slot 1: one press
- slot 2: two presses
- slot 3: three presses
- slot 4: four presses

This button sequence is part of the physical interface. Explain each slot to users so they know what will be typed before they press the button.

## Output behavior

For static password, HOTP, and Yubico OTP modes, the device can emit characters through the USB keyboard interface.

Challenge-response is different:

- it is not meant to "type" a value
- it computes a response from a host-supplied challenge

Explain this distinction to users who expect every slot to type its output.

## When these slots are a good fit

They make sense when you need compatibility with:

- older systems that still expect OTP
- workflows already built around Yubico-style slot semantics
- challenge-response integrations that are not WebAuthn

Use these slots for compatibility with older systems; passkeys remain the better choice for phishing-resistant authentication.

## Security cautions

- static passwords are only as safe as the place they are typed
- OTP keyboard emission is vulnerable to input focus mistakes
- physical possession may be enough to trigger output if extra controls are not enabled

If the goal is phishing-resistant authentication, use passkeys and keep slot features only where legacy compatibility forces the issue.
