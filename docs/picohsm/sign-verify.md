# Sign and verify — Digital signatures

This document describes how to sign and verify data using keys stored in Pico HSM.
The content is directly based on the official Pico HSM documentation.

## Overview

Pico HSM supports digital signatures using asymmetric keys.

Supported signature types include:

- RSA signatures
- ECDSA signatures

Signing operations are executed inside the device, and private keys never leave the HSM.

---

## Prepare test data

Create a test input file:

```bash
echo "This is a test string. Be safe, be secure." > data
```
---

## RSA signatures

### Export public key

First, export the RSA public key from Pico HSM:

```bash
pkcs11-tool --read-object --type pubkey --id 1 --output-file rsa_pub.der
```

Convert it to PEM format:

```bash
openssl rsa -inform DER -pubin -in rsa_pub.der -out rsa_pub.pem
```

### Sign data with RSA

Sign the data using the private key stored in Pico HSM:

```bash
pkcs11-tool \
  --sign \
  --id 1 \
  --pin 648219 \
  --mechanism RSA-PKCS \
  -i data \
  -o data.sig
```

!!! warning
    RSA-PKCS is considered deprecated for new designs. Use RSA-PSS when possible.

### Verify RSA signature

Verify the signature using OpenSSL:

```bash
openssl dgst -sha256 \
  -verify rsa_pub.pem \
  -signature data.sig \
  data
```

If the signature is valid, OpenSSL will report success.

---

## ECDSA signatures

### Export EC public key

Export the EC public key from Pico HSM:

```bash
pkcs11-tool --read-object --type pubkey --id 11 --output-file ec_pub.der
```

Convert it to PEM:

```bash
openssl ec -inform DER -pubin -in ec_pub.der -out ec_pub.pem
```

### Sign data with ECDSA

Sign the data using the EC private key stored in Pico HSM:

```bash
pkcs11-tool \
  --sign \
  --id 11 \
  --pin 648219 \
  --mechanism ECDSA \
  -i data \
  -o data.sig
```

### Verify ECDSA signature

Verify the ECDSA signature using OpenSSL:

```bash
openssl dgst -sha256 \
  -verify ec_pub.pem \
  -signature data.sig \
  data
```

If the signature is valid, OpenSSL will confirm it.

### Notes on hashing

When using pkcs11-tool:

- Hashing may be performed internally by the mechanism
- Alternatively, the host may hash the data before signing

!!! note
    Ensure that the hashing strategy used for signing and verification is consistent.

---

## Summary

Using Pico HSM for digital signatures allows you to:

- Sign data with RSA or ECDSA keys stored securely
- Verify signatures using standard OpenSSL tools
- Keep private keys fully isolated inside the HSM

All cryptographic signing operations are performed inside the device.
