# Asymmetric ciphering

Pico HSM exposes two different ideas under this heading:

- RSA decryption
- ECDH shared-secret derivation

Those are related only in the sense that both use asymmetric keys.

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

The documentation should be blunt here:

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

If `cmp` prints nothing, the derived secrets match.

## Practical caution

Asymmetric ciphering is where users often discover that:

- the firmware is capable
- the host tool syntax is obscure
- the intended application still needs adaptation

That is normal. Test the full application path, not just the raw cryptographic primitive.
