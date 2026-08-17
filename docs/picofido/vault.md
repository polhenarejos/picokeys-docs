# Pico Vault

Pico Vault is an opt-in Pico FIDO feature for moving selected resident
credentials between enrolled Pico FIDO devices. The exported object is an
authenticated, opaque package; the normal host workflow does not receive the
credential private key in plaintext.

Vault is a vendor extension. It is separate from ordinary WebAuthn, CTAP, and
browser passkey behavior.

## Research reference

The design is described in Pol Henarejos, *Vaulted Passkeys: A Device-Bound
Proposal for Authenticated Credential Export and Import* (2026), available on
[arXiv](https://arxiv.org/abs/2608.13806).

```bibtex
@misc{henarejos2026vaultedpasskeysdeviceboundproposal,
      title={Vaulted Passkeys: A Device-Bound Proposal for Authenticated Credential Export and Import},
      author={Pol Henarejos},
      year={2026},
      eprint={2608.13806},
      archivePrefix={arXiv},
      primaryClass={cs.CR},
      url={https://arxiv.org/abs/2608.13806},
}
```

## Requirements

- Pico FIDO firmware with Vault support
- a registered PicoKeys board and its PIN
- the standalone [`pico-vault-enroller`](https://github.com/polhenarejos/pico-vault-enroller)
- a license file and network access when requesting the Vault certificate
- physical access to the board's `BOOTSEL` button

The enroller provisions the Vault. It does not flash firmware, export
credentials, or import credentials.

## Enroll a board

Clone the enroller and install it in an isolated Python environment, then
create an encrypted enrollment envelope:

```sh
git clone https://github.com/polhenarejos/pico-vault-enroller
cd pico-vault-enroller
python3 -m venv .venv
. .venv/bin/activate
python -m pip install .
pico_vault_enroller create \
  --license-file /secure/path/license.json \
  --label "office backup"
```

Keep the envelope and its passphrase separately. Enroll the board with the
same envelope:

```sh
pico_vault_enroller enroll \
  --license-file /secure/path/license.json \
  --envelope /secure/path/enrollment.json
```

The command requests the board PIN, asks you to disconnect and reconnect the
board, and then waits while you hold `BOOTSEL` for the enrollment ceremony.
The printed Vault ID should be recorded with the board inventory.

The GUI equivalent is:

```sh
pico_vault_enroller gui
```

The full CLI and replacement-board procedure are in the enroller's
[tutorial](https://github.com/polhenarejos/pico-vault-enroller/blob/main/docs/tutorial.md)
and [operations guide](https://github.com/polhenarejos/pico-vault-enroller/blob/main/docs/operations.md).

## Export and import

With the board enrolled, use the [PicoKey App Vault panel](../picokeyapp/fido/vault.md)
to:

1. unlock the FIDO passkey session;
2. export selected resident credentials to the local Vault store;
3. move the enrollment envelope to a replacement board and enroll it into the
   same Vault; and
4. import the selected credentials on that board.

The Vault workflow preserves credential metadata that cannot be reconstructed
from the private key alone. Imported credentials remain distinct from native
credentials and do not copy the destination device's native signature counter.

Four authenticated encryption profiles are available:

| Profile | Use |
| --- | --- |
| ChaChaPoly | single authenticated encryption layer |
| AES-GCM | single authenticated encryption layer |
| ChaChaPoly + AES-GCM | two authenticated layers |
| AES-GCM + ChaChaPoly | two authenticated layers in the opposite order |

The local Vault store may contain credentials from multiple Vault IDs. They are
grouped by Vault ID, but only credentials belonging to the active Vault can be
imported. Credentials from another Vault can be displayed in the local store
but cannot be imported into the current board.

The same enrollment envelope can be used to enroll multiple boards into one
Vault domain. Those boards report the same Vault ID and can exchange exported
credentials. Create a new envelope only when a separate Vault domain is
intended.

## Recovery and retirement

The enrollment JSON is an encrypted recovery copy of the Vault material. It is
not replaceable by the board PIN. Losing its passphrase makes the file
unusable; losing both the board copy and the recovery copy prevents recovery
of credentials in that Vault domain.

To use a replacement board, reuse the original enrollment JSON and verify that
the new board reports the same Vault ID. Creating a new envelope creates a new
Vault domain.

To remove Vault material from a board:

```sh
pico_vault_enroller unenroll --pin '…'
```

The command removes the Vault key and certificate from the board but does not
delete the local enrollment JSON. Treat an exposed envelope, passphrase, PIN,
or exported package as a recovery-asset incident and rotate the Vault domain.
