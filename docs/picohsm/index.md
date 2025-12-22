# Pico HSM

Pico HSM is a firmware variant that turns a PicoKeys device into a **hardware security module**.

It is designed to securely store cryptographic material and to perform cryptographic operations **inside the device**, without exposing private keys to the host system.

---

## Purpose

The main goals of Pico HSM are:

- Secure key storage
- Hardware-backed cryptographic operations
- Isolation of private keys from the host system
- Integration with standard cryptographic interfaces

!!! note
    Pico HSM defines device behavior. PicoKeyApp is only used to manage and provision the device.

---

## What Pico HSM is

Pico HSM is:

- A firmware running on a PicoKeys device
- A hardware-backed cryptographic endpoint
- A solution for protecting long-term cryptographic keys
- A component designed to work with external software via standard APIs

---

## What Pico HSM is not

Pico HSM is not:

- A general-purpose smartcard emulator
- A software-only cryptographic library
- A replacement for full enterprise-grade HSMs
- A key backup or recovery service

!!! warning
    Loss of the device may result in permanent loss of stored private keys.

---

## Key security properties

Pico HSM enforces the following security properties:

- Private keys never leave the device
- Cryptographic operations are executed internally
- Key usage is restricted by firmware policy
- Persistent storage is protected by the device firmware

!!! note
    Security guarantees depend on correct usage and threat assumptions.

---

## Supported cryptographic operations

The exact set of supported operations depends on the firmware version, but typically includes:

- Key generation
- Digital signatures
- Signature verification
- Encryption and decryption
- Key agreement

!!! note
    Supported algorithms and curves are firmware-specific and may evolve over time.

---

## Interfaces and integration

Pico HSM is designed to integrate with host systems through **standard interfaces**.

Typical integrations include:

- PKCS#11-compatible software
- Cryptographic middleware
- Custom applications using supported APIs

!!! tip
    Using standard interfaces simplifies integration and reduces the need for custom tooling.

---

## Host system requirements

To use Pico HSM effectively, the host system typically requires:

- A supported operating system
- Appropriate user permissions for USB access
- Required middleware or libraries (e.g. PKCS#11)

!!! note
    Host-side setup is outside the scope of PicoKeyApp and depends on the operating system.

---

## Firmware lifecycle

Using Pico HSM involves several stages:

- Firmware installation
- Board registration
- Device commissioning
- Operational use via host software

!!! warning
    Reinstalling or changing firmware may reset configuration or require re-commissioning.

---

## Limitations

Pico HSM has intentional limitations, including:

- No key export functionality
- No built-in key escrow or recovery
- Finite storage capacity
- Limited concurrency compared to large HSMs

!!! danger
    Keys stored on the device should be considered non-recoverable if the device is lost or damaged.

---

## Typical use cases

Pico HSM is suitable for use cases such as:

- Protecting signing keys
- Hardware-backed authentication
- Secure automation credentials
- Development and testing of HSM-based workflows

It is not intended for:

- High-throughput enterprise HSM workloads
- Multi-tenant key hosting
- Regulatory-certified HSM deployments

---

## Next steps

After installing Pico HSM firmware:

- Review the supported features and algorithms
- Configure host-side integration
- Understand the security model and limitations

!!! tip
    Always test workflows with non-critical keys before deploying to production.
