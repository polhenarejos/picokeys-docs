# OpenPGP card

Pico OpenPGP exposes an OpenPGP card applet over USB CCID. The normal model is the same as other OpenPGP cards: signature, decryption, and authentication roles controlled by user and admin credentials.

## Check the card

Start with:

```sh
gpg --card-status
```

If this does not work, fix PC/SC, CCID, or scdaemon setup before trying key generation.

## PINs

The OpenPGP card model uses:

| Credential | Role |
| --- | --- |
| `PW1` | user operations: sign, decrypt, authenticate |
| `PW3` | admin operations: key generation, import, settings |
| reset code | recovery path for blocked user PIN, if configured |

Change defaults before real use:

```sh
gpg --card-edit
gpg/card> admin
gpg/card> passwd
```

## Generate keys on-card

```sh
gpg --card-edit
gpg/card> admin
gpg/card> key-attr
gpg/card> generate
```

`key-attr` is selected per role. Use it deliberately:

- signing key
- decryption key
- authentication key

Upstream claims RSA, ECDSA, ECDH, Brainpool, and `secp256k1` support. Host tool behavior can still vary, so validate with the exact GnuPG/OpenSC stack you plan to use.

## Import existing keys

If you need an offline master-key workflow, import subkeys instead:

```sh
gpg --expert --edit-key YOURKEY
gpg> toggle
gpg> key 1
gpg> keytocard
gpg> save
```

This keeps the operational trade-off explicit:

- on-card generation minimizes exposure
- import lets you maintain an off-card recovery story

## Daily use

Signing:

```sh
echo hi | gpg --clearsign
```

Decryption:

```sh
gpg --decrypt file.gpg
```

SSH through `gpg-agent`:

```sh
echo enable-ssh-support >> ~/.gnupg/gpg-agent.conf
gpgconf --kill gpg-agent
gpg --export-ssh-key YOURKEY > ~/.ssh/id_openpgp_card.pub
```

## UIF / touch policy

Upstream claims User Interaction Flag support. If enabled for a slot, PIN alone is not enough; the device also requires physical confirmation.

Use it where silent signing or decryption by a compromised host is a concern, and document it so operators understand the wait.

## Reset

A factory reset of the OpenPGP applet should be treated as destructive:

- keys are wiped
- PIN state is reset
- card metadata and recovery material may be reset

Test it only on non-critical material unless you are intentionally offboarding the card.
