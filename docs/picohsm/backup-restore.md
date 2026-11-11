# Backup and restore

Pico HSM backs up private keys by wrapping them under a DKEK-based scheme and
moving them under controlled conditions. It does not export private keys as
plaintext.

## Core idea

The upstream model relies on:

- a Device Key Encryption Key (DKEK)
- wrapped export
- controlled unwrap on a compatible destination

Before relying on backups, make sure you understand how DKEK shares are created, stored, and recovered.

## Generate or provision the DKEK path

The simplified example is:

```sh
pkcs11-tool \
  --generate-key \
  --key-type aes:32 \
  --label dkek \
  --id 01 \
  --pin 648219
```

For real deployments, the upstream project also describes share-based and threshold workflows; document those procedures in an operational runbook.

## Export a wrapped key

```sh
pkcs11-tool \
  --export-key \
  --id 12 \
  --wrap \
  --wrapping-key 01 \
  --pin 648219 \
  --output-file key-backup.bin
```

The output is still sensitive. It is just not plaintext key material.

## Restore the wrapped key

```sh
pkcs11-tool \
  --import-key \
  --unwrap \
  --wrapping-key 01 \
  --pin 648219 \
  --input-file key-backup.bin \
  --id 12 \
  --label restored-key
```

Whether this succeeds depends on compatible policy, compatible capability, and the correct DKEK context.

## What can go wrong

- DKEK shares are lost
- the destination device is not prepared correctly
- the team thinks "we have export files" means "we have tested recovery"

The third problem is avoidable: restore a backup on a test device before relying on it.

!!! danger
    A backup workflow that has never been restored on a test device is not a backup workflow. It is a hypothesis.
