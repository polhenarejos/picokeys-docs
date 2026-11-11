# FIDO Vault

The **FIDO > Vault** panel manages local backups of credentials exported from
an enrolled Pico FIDO device. It is the desktop part of the [Pico Vault
workflow](../../picofido/vault.md); it does not enroll a board.

![FIDO Vault panel](../../assets/images/picokeyapp/fido/vault.png)

## Before opening the panel

1. Register the board in PicoKey App.
2. Install and run the standalone `pico-vault-enroller`.
3. Enroll the board with an encrypted enrollment envelope.
4. Unlock **FIDO > Passkeys** with the device PIN.

Select **FIDO > Vault** and click **Refresh**. The panel shows the Vault ID
when the board is enrolled. If it is not enrolled, the panel points to the
standalone enroller.

The local store can contain packages from several Vault IDs. The panel groups
them by ID and shows the active board's Vault ID at the top.

## Export a passkey

From **FIDO > Passkeys**, choose one or more resident credentials and select an
export profile:

- `ChaChaPoly`
- `AES-GCM`
- `ChaChaPoly + AES-GCM`
- `AES-GCM + ChaChaPoly`

Use the credential's Vault export action or **Export selected**. The app saves
the opaque Vault package and display metadata in its local Vault store. A
credential already stored in the active Vault is not exported again.

The local store can display the relying party, user information, algorithm,
board serial, and Vault ID. It does not turn the package into a plaintext
private key.

## Import a passkey

On the destination board:

1. Enroll it with the same enrollment envelope.
2. Unlock **FIDO > Passkeys**.
3. Open **FIDO > Vault** and click **Refresh**.
4. Select a credential and use its import button, or choose **Import selected**.

You can import only packages whose Vault ID matches the active board. Packages
from other Vaults remain visible in their groups, but their import actions are
disabled. Imported credentials appear in the destination's passkey list as
separate resident credentials.

You can use the same encrypted enrollment envelope for multiple boards. They
will report the same Vault ID and can exchange exported credentials through
that Vault domain. Create another envelope only when you want a separate Vault
domain.

Use **Select all** and **Import selected** for a bulk import from the active
Vault group. The panel also provides an import button for each eligible row.

## Delete local Vault records

The row delete button and **Delete selected** remove packages from the local
Vault store. They leave any credential already imported into a board in place,
so deleting a package affects the backup store rather than the device.

## Security and recovery

Protect the enrollment JSON, its passphrase, device PIN, and exported packages
separately. PicoKey App cannot reset a lost enrollment passphrase. If a package
or enrollment secret is exposed, treat it as a recovery incident and rotate
the Vault domain according to the [Vault operations guide](https://github.com/polhenarejos/pico-vault-enroller/blob/main/docs/operations.md).
