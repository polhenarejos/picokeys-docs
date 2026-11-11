# SSH keys

Pico FIDO can be used with OpenSSH security-key keys: `ed25519-sk` and `ecdsa-sk`.

The authenticator performs private-key operations. The file on disk is only a handle that lets OpenSSH find the device, so it cannot be used without Pico FIDO.

## Requirements

- OpenSSH with FIDO support
- libfido2 or an equivalent security-key provider
- a visible Pico FIDO device
- a working button / user-presence path on the board profile you use

Check whether your SSH client knows security-key key types:

```sh
ssh -Q key | grep sk
```

On macOS, Apple's system OpenSSH may be missing a required provider path. If enrollment fails before the device is touched, try a current OpenSSH build before investigating the firmware.

## Enroll a key

```sh
ssh-keygen -t ed25519-sk -f ~/.ssh/id_ed25519_sk -C "you@laptop"
```

Use `ecdsa-sk` if the client or server rejects `ed25519-sk`:

```sh
ssh-keygen -t ecdsa-sk -f ~/.ssh/id_ecdsa_sk -C "you@laptop"
```

The resulting files have different roles:

- `id_ed25519_sk` is the handle file
- `id_ed25519_sk.pub` is the public key for `authorized_keys`

Copy the handle file to each machine you use for SSH; the server needs only the public key.

## Common enrollment options

Useful `ssh-keygen -O` options:

| Option | Effect |
| --- | --- |
| `resident` | store a discoverable SSH credential on the device |
| `verify-required` | require the FIDO PIN for each login |
| `application=ssh:NAME` | create a distinct application-scoped credential |
| `user=NAME` | set the user handle for resident keys |
| `write-attestation=FILE` | save enrollment attestation data |

Example:

```sh
ssh-keygen -t ed25519-sk \
  -O resident \
  -O verify-required \
  -O application=ssh:work \
  -f ~/.ssh/id_work_sk
```

## Login

Install the public key on the server:

```sh
ssh-copy-id -i ~/.ssh/id_ed25519_sk.pub you@server
```

Then connect:

```sh
ssh -i ~/.ssh/id_ed25519_sk you@server
```

The authenticator may require user presence, and it will also request a PIN when the key or relying policy requires user verification.

## Resident keys

Resident keys are useful when you want to recover the handle on another machine:

```sh
ssh-keygen -K
```

They consume discoverable credential storage. For most users, non-resident keys plus a backup or re-enrollment plan are simpler.

## SSH config

```sshconfig
Host server
    HostName server.example.com
    User you
    IdentityFile ~/.ssh/id_ed25519_sk
    IdentitiesOnly yes
```

`IdentitiesOnly yes` tells OpenSSH to skip unrelated keys and offer the hardware-backed key directly.

## Troubleshooting

- `requested feature not supported`: test `ecdsa-sk`, check OpenSSH/libfido2 support, and confirm the device is visible.
- no touch prompt: the client may not be reaching the authenticator.
- repeated PIN prompts: check whether both `ssh-agent` and an explicit `IdentityFile` are offering the same credential.
- old handle stops working after reset: the authenticator identity changed; restore the relevant seed if your workflow supports it, or enroll a new key.
