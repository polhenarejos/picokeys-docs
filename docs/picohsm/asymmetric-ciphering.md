# Asymmetric encryption and decryption

These instructions follow the **Asymmetric encryption/decryption** examples from the official Pico HSM docs.

Pico HSM supports:

- RSA decryption with several padding schemes
- ECDH shared key derivation

!!! note
    Ed25519/Ed448 (EdDSA) keys are intended for digital signatures, not for encryption/decryption flows.

The examples below show how to encrypt and decrypt with RSA and derive shared keys with ECDH.

---

## Prepare test data

Create a test input file:

```bash
echo "This is a test string. Be safe, be secure." > data
```

---

## RSA-PKCS (deprecated)

This padding scheme is insecure and deprecated.

First, extract the public key to PEM format:

```bash
pkcs11-tool --read-object --pin 648219 --id 1 --type pubkey > 1.der
openssl rsa -inform DER -outform PEM -in 1.der -pubin > 1.pub
```

Encrypt it using OpenSSL:

```bash
openssl rsautl -encrypt -inkey 1.pub -in data -pubin -out data.crypt
```

Then decrypt using Pico HSM:

```bash
pkcs11-tool \
  --id 1 \
  --pin 648219 \
  --decrypt \
  --mechanism RSA-PKCS \
  -i data.crypt
```

The output will show the original data.

---

## RSA-X-509

This padding requires the plaintext to be padded up to the key size in bytes.

Copy and pad the file:

```bash
cp data data_pad
dd if=/dev/zero bs=1 count=227 >> data_pad
```

Encrypt the padded data:

```bash
openssl rsautl -encrypt \
  -inkey 1.pub \
  -in data_pad \
  -pubin \
  -out data.crypt \
  -raw
```

Decrypt using Pico HSM:

```bash
cat data.crypt | pkcs11-tool \
  --id 4 \
  --pin 648219 \
  --decrypt \
  --mechanism RSA-X-509
```

!!! note
    In RSA-X-509 mode the plaintext must match the key length byte count before encryption.

---

## RSA-PKCS-OAEP (recommended)

OAEP provides proper padding with SHA256:

Encrypt with OpenSSL:

```bash
openssl pkeyutl -encrypt \
  -inkey 1.pub \
  -pubin \
  -pkeyopt rsa_padding_mode:oaep \
  -pkeyopt rsa_oaep_md:sha256 \
  -pkeyopt rsa_mgf1_md:sha256 \
  -in data \
  -out data.crypt
```

Decrypt inside Pico HSM:

```bash
pkcs11-tool \
  --id 1 \
  --pin 648219 \
  --decrypt \
  --mechanism RSA-PKCS-OAEP \
  -i data.crypt
```

!!! tip
    RSA-OAEP with SHA256 is strongly preferred over RSA-PKCS.

---

## ECDH shared secret derivation

ECC does not allow direct encryption; instead use ECDH to derive a shared secret:

Create a remote party keypair (Bob):

```bash
openssl ecparam -genkey -name prime192v1 > bob.pem
openssl ec -in bob.pem -pubout -outform DER > bob.der
```

Derive the shared secret:

```bash
pkcs11-tool \
  --pin 648219 \
  --id 11 \
  --derive \
  -i bob.der \
  -o mine-bob.der
```

Compute Bob’s shared secret locally:

```bash
openssl pkeyutl -derive \
  -out bob-mine.der \
  -inkey bob.pem \
  -peerkey 11.pub
```

Compare:

```bash
cmp bob-mine.der mine-bob.der
```

If nothing is printed, both keys match.

---

## Summary

This covers the main asymmetric ciphering flows in Pico HSM:

- RSA (various padding schemes)
- ECDH key agreement

All private key operations occur inside the device; keys are never exposed externally.
