# Capability map

The upstream README groups Pico HSM capabilities by use. These are firmware
claims, and generic host tools may expose only part of the feature set.

## Key generation and storage

Upstream claims support for:

- RSA key generation from 1024 to 4096 bits
- ECDSA key generation across common NIST and Brainpool curves
- Ed25519 and Ed448
- AES keys of 128, 192, 256, and for XTS-style cases effectively 512-bit concatenated material
- encrypted storage under an MKEK

Private keys are intended to remain on the device.

## Signatures and authentication

The advertised signature surface includes:

- RSA-PSS
- RSA-PKCS#1 v1.5
- raw RSA
- ECDSA
- EdDSA
- HMAC
- CMAC

There is also support for public-key authentication and usage counters.

## Encryption, decryption, and derivation

The upstream README lists support for:

- RSA decryption
- ECDH key derivation
- EC private-key derivation
- AES in ECB, CBC, CFB, OFB, XTS, CTR, GCM, and CCM
- ChaCha20-Poly1305
- HKDF, PBKDF2, and X9.63 KDF

Because this is a broad feature set for a microcontroller-based device, test each mechanism you plan to use with your chosen host tools.

## Wrapped-key and domain workflows

Pico HSM's more distinctive features include:

- DKEK shares
- key domains
- n-of-m threshold workflows
- wrapped-key export and import
- XKEK-style sharing schemes

These workflows offer more control, although generic PKCS#11 frontends are less likely to expose them fully.

## Data and metadata

Upstream also claims support for:

- arbitrary binary data storage
- certificates
- attestation objects
- RTC-backed time functions

The device can therefore store some supporting data alongside keys, although it is not a general file store.

## Transport and platform features

The README also advertises:

- USB CCID
- PKCS#11
- secure messaging
- secure boot
- secure lock
- rescue interfaces
- LED customization

Those transport and platform features matter as much operationally as the algorithms themselves.

## Conservative reading

The more advanced the feature, the more you should verify:

- whether the firmware implements it on your version
- whether the middleware exposes it
- whether the client you intend to use can drive it without custom tooling

Checking each layer before deployment helps avoid surprises.

## Storage and authorization changes

Current firmware also hardens the operational boundary around the feature set:

- HSM key objects use authenticated container records with legacy-layout
  compatibility;
- key-domain mutation, master-seed generation, and sensitive object changes
  require the appropriate authenticated session;
- usage counters are persisted and enforced across symmetric and derivation
  operations; and
- protected filesystem capacity and indexing have been expanded for larger
  stores.

These are firmware guarantees, not new PKCS#11 commands. Continue to validate
the exact host tool and recovery workflow you depend on.
