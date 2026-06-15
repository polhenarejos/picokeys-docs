# Public key authentication

Public-key authentication lets Pico HSM gate access with a challenge-response flow instead of relying only on an interactive PIN entry pattern.

That makes it useful for automation, but only if you model the trust boundary correctly.

## High-level flow

1. generate or provision an authentication key
2. export the public part
3. sign a challenge with the private key in the device
4. verify the signature with the public key

If verification succeeds, the holder of the device-controlled private key is authenticated.

## Generate an authentication key

```sh
pkcs11-tool \
  --keygen \
  --key-type EC:prime256v1 \
  --id 20 \
  --label auth-key \
  --pin 648219
```

## Export the public key

```sh
pkcs11-tool \
  --read-object \
  --type pubkey \
  --id 20 \
  --output-file auth_pub.der

openssl ec -inform DER -pubin -in auth_pub.der -out auth_pub.pem
```

## Sign a challenge

```sh
pkcs11-tool \
  --sign \
  --id 20 \
  --mechanism ECDSA \
  --pin 648219 \
  -i challenge.bin \
  -o response.sig
```

## Verify it

```sh
openssl dgst -sha256 -verify auth_pub.pem -signature response.sig challenge.bin
```

## Operational caution

This is powerful for headless use, but do not overstate it:

- if the automation environment is compromised, it may still be able to invoke authorized operations
- the public-key path removes some PIN friction, not the need for trust boundaries
- key management for the authentication key now becomes part of your automation security story

Use it where non-interactive access is genuinely needed, not as a reflex replacement for PIN-gated workflows.
