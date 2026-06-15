# Threat model

This page states what the PicoKeys firmware families can reasonably protect against and where the boundary ends.

It is intentionally conservative.

## Assets

Depending on firmware, the assets include:

- FIDO master seed
- resident passkeys
- OATH secrets
- OTP slot secrets
- OpenPGP private keys
- PINs and role credentials
- HSM keys and DKEK material

## Host attacker

Everything arriving over USB should be treated as attacker-controlled if the host is compromised.

Defenses include:

- PIN or user verification
- physical confirmation
- OpenPGP role separation
- retry counters and lockouts
- applet-specific management keys or policy gates

What remains true:

- a compromised host can request operations while the device is connected
- if the user authorizes an operation, the device may perform it
- touch confirms presence, not intent for every byte in the request

## Powered-off theft

At-rest protection depends heavily on hardware:

- RP2040 is weaker
- RP2350/RP2354 and ESP32-S3 class devices provide stronger primitives
- OTP-backed sealing and secure boot are meaningful only after correct provisioning

Do not describe a device as hardened merely because the firmware supports hardening in theory.

## Firmware replacement

Secure boot and lock mechanisms can prevent unauthorized firmware from running, but only when enabled correctly on hardware that supports them.

Anti-rollback is a separate concern. It prevents older signed firmware from being accepted after the owner raises the rollback floor.

## Physical attacks

Microcontroller boards are not secure elements.

Out of scope:

- decapping
- microprobing
- advanced fault injection
- power and EM side channels
- invasive lab extraction

If your threat model includes a funded lab, use certified secure hardware.

## Network

The firmware families documented here are USB-facing. There is no independent network stack in the device model.

Network exposure comes from the host or from services using the credentials, not from the device itself.

## Reporting and documentation discipline

When documenting a security claim, include:

- the firmware family
- the board family
- the provisioning state
- the host tool path used to verify it

Without those four pieces, the claim is probably too vague.
