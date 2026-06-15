# SSH keys

Pico FIDO can be used with OpenSSH security-key keys: `ed25519-sk` and `ecdsa-sk`.

The private key operation is performed by the authenticator. The file stored on disk is a handle that points OpenSSH back to the device; it is not useful by itself without the Pico FIDO device.

## Requirements

- OpenSSH with FIDO support
- libfido2 or an equivalent security-key provider
- a visible Pico FIDO device
- a working button / user-presence path on the board profile you use

Check whether your SSH client knows security-key key types:

```sh
ssh -Q key | grep sk
```

On macOS, Apple's system OpenSSH may lag or omit the needed provider path. If enrollment fails before the device is touched, test with a current OpenSSH build before blaming the firmware.

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

Copy the handle file only to machines you SSH from. The server only needs the public key.

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

Expect user presence, and expect a PIN as well if the key or relying policy requires user verification.

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

`IdentitiesOnly yes` avoids OpenSSH offering a pile of unrelated keys before it reaches the hardware-backed one.

## Troubleshooting

- `requested feature not supported`: test `ecdsa-sk`, check OpenSSH/libfido2 support, and confirm the device is visible.
- no touch prompt: the client may not be reaching the authenticator.
- repeated PIN prompts: check whether both `ssh-agent` and an explicit `IdentityFile` are offering the same credential.
- old handle stops working after reset: the authenticator identity changed; restore the relevant seed if your workflow supports it, or enroll a new key.
