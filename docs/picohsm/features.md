# Supported features — Pico HSM

This page lists the main cryptographic and operational capabilities provided by the **Pico HSM firmware**.

The features described here are supported by the firmware running on a PicoKeys device; availability may vary depending on firmware version and build configuration.

---

## Cryptographic key generation

Pico HSM supports generation and secure encrypted storage of private and secret keys inside the device.

!!! note
    Keys are encrypted using a master key (MKEK) protected by a PIN-derived secret. Private keys never leave the device in plaintext.

Supported key types include:

- **RSA key generation**
  RSA keys can be generated with sizes from **1024 bits up to 4096 bits**.
  Private keys always remain on the device.

- **ECDSA key generation**
  Supported curves include (but may not be limited to):
  secp192r1, secp256r1, secp384r1, secp521r1, brainpoolP256r1, brainpoolP384r1, brainpoolP512r1, secp192k1, and secp256k1.

- **EdDSA key generation**
  Supported Edwards curves include:
  Ed25519 and Ed448.

!!! warning
    Some curves (e.g., secp192k1) are considered insecure. Only use curves appropriate for your threat model.

---

## Digital signatures

Pico HSM can perform signatures using generated keys:

- **RSA signatures**
  Supports multiple RSA signature algorithms including:
    - RSA-PSS
    - RSA-PKCS
    - Raw RSA signatures

- **ECDSA signatures**
  ECDSA signatures may be generated in raw or pre-hashed form.

- **EdDSA signatures**
  EdDSA signatures are supported with Ed25519 and Ed448 keys.

---

## Key agreement and derivation

Pico HSM supports:

- **ECDH (Elliptic Curve Diffie–Hellman)** for shared secret derivation.
- **EC private key derivation.**

---

## Encryption and decryption

Symmetric and asymmetric encryption capabilities include:

- **RSA decryption**
    - RSA-OEP
    - RSA-X.509 decryption

- **AES key generation**
    - AES keys of 128, 192, and 256 bits

- **Advanced AES modes**
    - ECB, CBC, CFB, OFB, XTS, CTR, GCM, and CCM with customizable IV/nonce and authenticated data.

---

## Message authentication and hashing

- **CMAC (AES-CMAC)** authentication.

- **HMAC and hash functions**
  PCM supports multiple hash digest algorithms (SHA-1, SHA-224, SHA-256, SHA-384, SHA-512) for signing, digesting, and derivation operations.

---

## PKCS#11 and interface support

- **PKCS#11 compliant interface**
  Pico HSM supports a PKCS#11 interface for integration with standard middleware and host applications.

- **USB/CCID support**
  Implements a USB CCID stack for host communication (e.g., using OpenSC, OpenSSL tools).

- **Extended APDU support** (up to 65535 bytes).

---

## Hardware and cryptographic support

- **Hardware Random Number Generator (HRNG)** – for high-entropy random values.

- **PIN authorization** – requiring a PIN prior to key usage.

---

## Advanced key management

Pico HSM supports advanced operational schemes:

- **Device Key Encryption Key (DKEK) shares** – enables import/export of wrapped keys.

- **n-of-m threshold schemes** – preventing outages when custodians are unavailable.

- **Master Key Encryption Key (MKEK)** – encrypted master used to protect stored keys.

---

## Storage and additional features

- **Arbitrary binary data storage** – allows storing keyed binary data.

- **Real-Time Clock (RTC)** – for date/time operations.

- **Key usage counter** – tracks and limits key usage.

---

## Security mechanisms

- **Press-to-confirm button** – if enabled, requires physical button press to authorize private operations (depends on firmware configuration).

- **Secure messaging / channel encryption** – protects data between host and device.

---

## Advanced firmware features

Depending on firmware build and version, Pico HSM may also support:

- **Secure Boot** – only authenticated firmware loads.

- **Secure Lock** – locks the device to specific firmware, preventing future unauthorized firmware installations.

!!! danger
    Features such as Secure Lock and Secure Boot may be **irreversible** once enabled on the device.

---

## Summary

Pico HSM provides a **wide range of cryptographic primitives, interfaces, and hardware security features**, suitable for use as a compact hardware security module on supported microcontrollers.
