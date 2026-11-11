# AES

Pico HSM supports a broad set of symmetric operations, although host tools expose them less consistently than signature operations.

## What upstream claims

The upstream README advertises:

- AES key generation
- AES-CBC
- extended modes including ECB, CBC, CFB, OFB, XTS, CTR, GCM, and CCM
- CMAC and related symmetric workflows

Many standard PKCS#11 frontends do not make this full range of modes easy to use.

## Generate an AES key

```sh
alias sc-tool="pkcs11-tool --module /path/to/libsc-hsm-pkcs11.so"

sc-tool \
  -l --pin 648219 \
  --keygen \
  --key-type AES:32 \
  --id 12 \
  --label AES32
```

The resulting object should be a secret AES key.

## Encrypt with AES-CBC

```sh
echo "This is a text." | sc-tool \
  -l --pin 648219 \
  --encrypt \
  --id 12 \
  --mechanism aes-cbc > crypted.aes
```

## Decrypt

```sh
cat crypted.aes | sc-tool \
  -l --pin 648219 \
  --decrypt \
  --id 12 \
  --mechanism aes-cbc
```

## Important caveats

- AES-CBC expects block-aligned input unless your toolchain handles padding externally
- the firmware may support more modes than your chosen host utility exposes cleanly
- authenticated modes such as GCM or CCM deserve end-to-end testing with your exact application stack

If your application depends on a particular mode, test that mode with the intended host tools before building around it.
