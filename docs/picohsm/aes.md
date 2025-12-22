# AES (Symmetric encryption/decryption)

These instructions are directly based on the official Pico HSM documentation. The examples use `sc-tool` and `pkcs11-tool` as shown in the upstream docs.

---

## Overview

Pico HSM supports:

- AES secret key generation
- AES encryption
- AES decryption

!!! note
    AES support in the standard OpenSC `sc-hsm` driver is limited, so examples below use `pkcs11-tool` or `sc-tool` with the proper module.

---

## Secret key generation

To generate a **256-bit AES secret key**, run:

```bash
alias sc-tool="pkcs11-tool --module /path/to/libsc-hsm-pkcs11.so"

sc-tool \
  -l --pin 648219 \
  --keygen \
  --key-type AES:32 \
  --id 12 \
  --label "AES32"
```

The output should indicate a secret key object:

```bash
Secret Key Object; AES length 32 label: AES32 ID: 12 Usage: encrypt, decrypt ...
```

For other key sizes:

- 128-bit: `aes:16`
- 192-bit: `aes:24`
- 256-bit: `aes:32`

!!! tip
    After generating the AES key, list keys with `sc-tool -l --pin <PIN> --list-object --type secrkey`.

---

## Encryption

To encrypt data using a generated AES key:

```bash
echo "This is a text." | sc-tool \
  -l --pin 648219 \
  --encrypt \
  --id 12 \
  --mechanism aes-cbc > crypted.aes
```

The ciphertext will be stored in `crypted.aes`.

!!! warning
    AES-CBC requires input length that is a multiple of 16 bytes. For short data, padding must be applied.

---

Decryption

To decrypt the ciphertext with the same key:

```bash
cat crypted.aes | sc-tool \
  -l --pin 648219 \
  --decrypt \
  --id 12 \
  --mechanism aes-cbc
```

The output will be the original plaintext.

!!! note
    AES decrypt is the inverse of encrypt; confirm the mechanism matches the original encryption.

## Summary

AES on Pico HSM allows you to:

- generate AES keys of 128/192/256 bits
- encrypt data with those keys
- decrypt data with matching keys

All operations are performed inside the device; secret keys never leave the secure module.
