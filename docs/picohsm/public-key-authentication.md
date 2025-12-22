# Public key authentication

This document describes how to perform public key authentication using Pico HSM, based directly on the official Pico HSM documentation.

Public key authentication allows a host to authenticate itself to the device without using a PIN, relying instead on cryptographic challenge–response.

## Overview

Public key authentication works by:

- Storing an authentication key inside Pico HSM
- Generating a random challenge on the device
- Signing the challenge using the private key
- Verifying the signature using the corresponding public key

!!! note
    This mechanism is useful for automated systems where interactive PIN entry is not possible.

## Generate an authentication key

First, generate an EC key pair that will be used for authentication.

```bash
pkcs11-tool \
  --keygen \
  --key-type EC:prime256v1 \
  --id 20 \
  --label auth-key \
  --pin 648219
```

This key will remain stored inside Pico HSM.

## Export the public key

Export the public part of the authentication key:

```bash
pkcs11-tool \
  --read-object \
  --type pubkey \
  --id 20 \
  --output-file auth_pub.der
```

Convert it to PEM format:

```bash
openssl ec \
  -inform DER \
  -pubin \
  -in auth_pub.der \
  -out auth_pub.pem
```

Authenticate using a challenge

Request a random challenge from Pico HSM and sign it using the authentication key.

```bash
pkcs11-tool \
  --sign \
  --id 20 \
  --mechanism ECDSA \
  --pin 648219 \
  -i challenge.bin \
  -o response.sig
```

!!! note
    The challenge data must match the data used during verification.

## Verify authentication response

Verify the signature using the public key:

```bash
openssl dgst \
  -sha256 \
  -verify auth_pub.pem \
  -signature response.sig \
  challenge.bin
```

If verification succeeds, authentication is considered valid.

---

## Authentication flow summary

The complete authentication process is:

- Device generates or receives a challenge
- Host signs the challenge using the authentication key
- Device or host verifies the signature
- Access is granted if verification succeeds

!!! tip
    Public key authentication can be combined with PIN-based access for layered security.

---

## Security considerations

When using public key authentication:

- Protect the authentication private key carefully
- Use secure storage for public keys
- Avoid reusing authentication keys across systems

!!! warning
    Compromise of the authentication private key allows impersonation.

---

## Summary

Public key authentication in Pico HSM provides:

- Non-interactive authentication
- Cryptographic challenge–response
- Reduced reliance on PIN entry
- Secure automation support

This mechanism is particularly useful in headless or automated environments.
