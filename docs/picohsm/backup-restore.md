# Backup and restore

Backup in Pico HSM does not mean "export the private key as plaintext." It means "wrap key material under a DKEK-based scheme and move it under controlled conditions."

That distinction is the whole point.

## Core idea

The upstream model relies on:

- a Device Key Encryption Key (DKEK)
- wrapped export
- controlled unwrap on a compatible destination

If you do not understand your DKEK story, you do not yet have a backup story.

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

In real deployments, upstream also describes share-based and threshold workflows. Those need operational runbooks, not just commands.

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

Only the last point is avoidable by discipline alone, and it is also the most common.

!!! danger
    A backup workflow that has never been restored on a test device is not a backup workflow. It is a hypothesis.
