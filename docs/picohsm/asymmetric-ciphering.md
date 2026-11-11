# Asymmetric ciphering

This section covers two asymmetric-key operations:

- RSA decryption
- ECDH shared-secret derivation

Both use asymmetric keys, but they serve different purposes: RSA decrypts data, while ECDH derives a shared secret.

## Prepare sample data

```sh
echo "This is a test string. Be safe, be secure." > data
```

## RSA-OAEP is the sane default

First export the public key:

```sh
pkcs11-tool --read-object --pin 648219 --id 1 --type pubkey > 1.der
openssl rsa -inform DER -outform PEM -in 1.der -pubin > 1.pub
```

Encrypt on the host:

```sh
openssl pkeyutl -encrypt \
  -inkey 1.pub \
  -pubin \
  -pkeyopt rsa_padding_mode:oaep \
  -pkeyopt rsa_oaep_md:sha256 \
  -pkeyopt rsa_mgf1_md:sha256 \
  -in data \
  -out data.crypt
```

Decrypt in Pico HSM:

```sh
pkcs11-tool \
  --id 1 \
  --pin 648219 \
  --decrypt \
  --mechanism RSA-PKCS-OAEP \
  -i data.crypt
```

## RSA-PKCS and RSA-X.509 exist, but carefully

Upstream examples also show `RSA-PKCS` and `RSA-X-509`.

Use the following guidance when choosing between these mechanisms:

- `RSA-PKCS` exists largely for compatibility
- `RSA-X-509` is low-level and easy to misuse
- neither should be your first recommendation when OAEP is available

## ECDH is not encryption

With elliptic-curve keys, the usual operation is shared-secret derivation:

```sh
openssl ecparam -genkey -name prime192v1 > bob.pem
openssl ec -in bob.pem -pubout -outform DER > bob.der

pkcs11-tool \
  --pin 648219 \
  --id 11 \
  --derive \
  -i bob.der \
  -o mine-bob.der
```

And compare against the host-side derived secret:

```sh
openssl pkeyutl -derive -out bob-mine.der -inkey bob.pem -peerkey 11.pub
cmp bob-mine.der mine-bob.der
```

When `cmp` prints nothing, the two derived secrets match.

## Practical caution

Asymmetric ciphering is where users often discover that:

- the firmware is capable
- the host tool syntax is obscure
- the intended application still needs adaptation

This is common with low-level cryptographic tools, so test the full application path as well as the primitive itself.
