# Slots and OTP

This page documents slot-based OTP behavior in Pico FIDO compatible modes.

---

## Slot model

Pico FIDO supports up to **4 slots**. Each slot stores one configured behavior.

The 4 supported slot schemes are:

- Static password
- HOTP
- Yubico OTP
- Challenge-response

Each slot is independent and can be provisioned or deleted separately.

Slot state model:

- Empty: no behavior configured
- Configured: one of the 4 schemes is active

---

## Activation with BOOTSEL

Slots are activated by pressing the BOOTSEL button `N` consecutive times, where `N` is the slot number:

- Slot 1: press once
- Slot 2: press twice
- Slot 3: press three times
- And so on

Presses must be consecutive so firmware can map the sequence to the intended slot.

---

## Keyboard-style output

When a slot is configured as:

- Static password
- HOTP
- Yubico OTP

Activation emits output through the USB HID keyboard interface, so the host receives characters as if typed by a keyboard.

!!! note
    Challenge-response does not emit keyboard text in this way; it returns response data through protocol operations.

---

## Operational differences by slot type

- Static password: emits fixed text.
- HOTP: emits a new counter-based OTP code each activation.
- Yubico OTP: emits Yubico-format OTP payload.
- Challenge-response: processes host-provided challenge and returns a computed response.

---

## Security notes

- Anyone with physical access may trigger slot output unless extra controls are enabled.
- Static passwords are convenient but weaker than modern passkeys.
- For phishing resistance, prefer WebAuthn passkeys and keep OTP as compatibility fallback.
