# Key Generation

##  Generate keys

Supported key families and curves include:

- RSA: 1024 to 4096 bits
- Weierstrass EC (ECDSA/ECDH): secp192r1, secp256r1, secp384r1, secp521r1, secp256k1, brainpoolP256r1, brainpoolP384r1, brainpoolP512r1
- Edwards curves: Ed25519 and Ed448

!!! note
    Exact algorithm availability depends on firmware version/build and PKCS#11 middleware support.

Generate an RSA key:

```bash
pkcs11-tool \
  --keygen \
  --key-type rsa:2048 \
  --id 01 \
  --label rsa-key \
  --pin 648219
```

or generate an EC key:

```bash
pkcs11-tool \
  --keygen \
  --key-type EC:prime256v1 \
  --id 11 \
  --label ec-key \
  --pin 648219
```

!!! note
    Key generation is performed entirely inside the device.

## Export public keys

Export an RSA public key:

```bash
pkcs11-tool \
  --read-object \
  --type pubkey \
  --id 01 \
  --output-file rsa_pub.der
```

Convert to PEM:

```bash
openssl rsa -inform DER -pubin -in rsa_pub.der -out rsa_pub.pem
```

## Sign data

Sign a file using a private key:

```bash
pkcs11-tool \
  --sign \
  --id 01 \
  --pin 648219 \
  --mechanism RSA-PKCS \
  -i data \
  -o data.sig
```

!!! warning
    RSA-PKCS is deprecated for new designs. Prefer RSA-PSS when available.

## Verify signatures

Verify using OpenSSL:

```bash
openssl dgst -sha256 \
  -verify rsa_pub.pem \
  -signature data.sig \
  data
```

## Encrypt and decrypt

Asymmetric and symmetric encryption are supported.

Refer to:

- [Asymmetric Ciphering](asymmetric-ciphering.md)
- [AES](aes.md)

for detailed workflows and examples.

## Backup and restore keys

Keys can be exported and imported in wrapped form.

Refer to:

[Backup & Restore](backup-restore.md)

!!! danger
    Loss of backup material or DKEK shares may make keys unrecoverable.

## Store arbitrary data

Store and retrieve small data blobs:

- Write data objects
- Read data objects
- Delete data objects

Refer to:

[Store data](store-data.md)

## Low-level debugging

For APDU-level interaction and debugging, use:

[Smart card shell 3](scs3.md)

Common issues

If commands fail:

- Verify the correct PIN
- Ensure the device is not locked
- Check USB permissions
- Confirm the correct PKCS#11 module path

!!! tip
    Running tools with increased verbosity can help diagnose issues.

---

## Summary

Pico HSM usage typically involves:

- Managing keys via PKCS#11
- Performing cryptographic operations inside the device
- Using standard tools such as OpenSSL and OpenSC
- Keeping private key material fully isolated

This workflow enables secure cryptographic operations without exposing sensitive keys to the host system.
