# Sign and verify

Signing is the most common proof that Pico HSM is working as intended: the host gets a signature, the private key stays inside the device.

## Prepare sample data

```sh
echo "This is a test string. Be safe, be secure." > data
```

## RSA signature flow

Export the public key:

```sh
pkcs11-tool --read-object --type pubkey --id 1 --output-file rsa_pub.der
openssl rsa -inform DER -pubin -in rsa_pub.der -out rsa_pub.pem
```

Sign:

```sh
pkcs11-tool \
  --sign \
  --id 1 \
  --pin 648219 \
  --mechanism RSA-PKCS \
  -i data \
  -o data.sig
```

Verify:

```sh
openssl dgst -sha256 -verify rsa_pub.pem -signature data.sig data
```

!!! warning
    `RSA-PKCS` is widely interoperable but not the first choice for new designs. Prefer RSA-PSS where your tooling supports it.

## ECDSA signature flow

Export the public key:

```sh
pkcs11-tool --read-object --type pubkey --id 11 --output-file ec_pub.der
openssl ec -inform DER -pubin -in ec_pub.der -out ec_pub.pem
```

Sign:

```sh
pkcs11-tool \
  --sign \
  --id 11 \
  --pin 648219 \
  --mechanism ECDSA \
  -i data \
  -o data.sig
```

Verify:

```sh
openssl dgst -sha256 -verify ec_pub.pem -signature data.sig data
```

## What to watch for

- host tools differ on whether hashing is implicit or explicit
- some mechanisms exist in firmware but are awkward in generic CLI tooling
- signature success proves the session, PIN, and key object are all aligned correctly

That makes sign-and-verify one of the best diagnostic workflows after initial provisioning.
