# Capability map

This page groups the Pico HSM feature set into operational categories. It is based on the upstream README and should be read as "firmware claims", not as "every generic tool exposes this equally well."

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

That is a large surface area for a microcontroller-based device. Test the specific mechanisms you need before assuming tool support is mature.

## Wrapped-key and domain workflows

Pico HSM's more distinctive features include:

- DKEK shares
- key domains
- n-of-m threshold workflows
- wrapped-key export and import
- XKEK-style sharing schemes

These are powerful, but they are also the part least likely to be well represented by generic PKCS#11 frontends.

## Data and metadata

Upstream also claims support for:

- arbitrary binary data storage
- certificates
- attestation objects
- RTC-backed time functions

This lets the device carry more than keys, but it does not turn it into a general file store.

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

That rule avoids most disappointment.

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
