# Git signing + auth

Pico FIDO can participate in two different Git workflows:

- SSH commit/tag signing
- SSH authentication for push and pull

Those are separate credentials even if they live on the same physical device.

## SSH signing

Git 2.34+ can sign commits and tags with SSH keys, including `-sk` hardware-backed keys.

```sh
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519_sk.pub
git config --global commit.gpgsign true
git config --global tag.gpgsign true
```

Then:

```sh
git commit -m "signed with Pico FIDO"
git log --show-signature -1
```

Each signature invokes the authenticator.

## Local verification

SSH signature verification needs an allowed signers file:

```sh
mkdir -p ~/.config/git
printf '%s namespaces="git" %s\n' \
  "you@example.com" \
  "$(cat ~/.ssh/id_ed25519_sk.pub)" \
  >> ~/.config/git/allowed_signers

git config --global gpg.ssh.allowedSignersFile ~/.config/git/allowed_signers
git verify-commit HEAD
```

If Git can sign but cannot verify locally, the allowed signers file is usually the missing piece.

## GitHub and GitLab signing

Add the public key as a signing key in the forge account settings.

Do not confuse this with an authentication key. Most forges treat signing keys and SSH login keys as separate account records.

## Push and pull over SSH

Use a hardware-backed SSH key as your transport key:

```sh
git remote set-url origin git@github.com:you/repo.git
git push
```

Each new SSH connection may require user presence, and possibly a PIN.

For fewer repeated prompts during a burst of Git operations, use SSH connection reuse:

```sshconfig
Host github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_sk
    IdentitiesOnly yes
    ControlMaster auto
    ControlPath ~/.ssh/cm-%r@%h:%p
    ControlPersist 10m
```

## Account 2FA

For browser-side account challenges, register Pico FIDO as:

- a passkey, if the forge supports it
- a security key / WebAuthn second factor otherwise

That is separate from commit signing and separate from SSH push/pull authentication.

## OpenPGP alternative

If you use Pico OpenPGP for a GnuPG identity, Git can sign through the OpenPGP card instead. See [OpenPGP card](../pico-openpgp/openpgp-card.md).

## Troubleshooting

- `git verify-commit` reports no trusted signature: configure `gpg.ssh.allowedSignersFile`.
- commits are unverified on a forge: add the key as a signing key and check the commit email.
- `git push` says public key denied: add the key as an authentication key and verify the remote uses SSH.
- too many touch prompts: check `ssh-agent`, `IdentityFile`, and connection reuse.
