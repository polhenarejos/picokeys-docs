# Key generation

Key generation is the cleanest Pico HSM workflow because it uses the device exactly as intended: create private material inside the device and export only what must be public.

## Supported families

According to upstream, Pico HSM supports:

- RSA: 1024 to 4096 bits
- Weierstrass EC: common NIST, Koblitz, and Brainpool curves
- Edwards curves: Ed25519 and Ed448
- AES secret keys

Exact mechanism names exposed through a host tool may still vary by middleware version.

## Generate an RSA key

```sh
pkcs11-tool \
  --keygen \
  --key-type rsa:2048 \
  --id 01 \
  --label rsa-key \
  --pin 648219
```

Use RSA only when you need it. Large RSA keys cost real time on this class of hardware.

## Generate an EC key

```sh
pkcs11-tool \
  --keygen \
  --key-type EC:prime256v1 \
  --id 11 \
  --label ec-key \
  --pin 648219
```

For many deployments, this is the practical default.

## Export the public part

For RSA:

```sh
pkcs11-tool \
  --read-object \
  --type pubkey \
  --id 01 \
  --output-file rsa_pub.der
openssl rsa -inform DER -pubin -in rsa_pub.der -out rsa_pub.pem
```

For EC:

```sh
pkcs11-tool \
  --read-object \
  --type pubkey \
  --id 11 \
  --output-file ec_pub.der
openssl ec -inform DER -pubin -in ec_pub.der -out ec_pub.pem
```

## Validate immediately

Do not stop at generation. Immediately:

- list the object
- export the public key
- sign or decrypt one test payload

That confirms the key is usable, not only present.

## Practical cautions

- RSA 3072 and 4096 are expensive enough to affect normal UX
- unsupported or oddly named curve identifiers in host tools can mislead users
- a generated key is only useful if your intended client stack can consume it later

That last point is why key generation should always be followed by one end-to-end application test.
