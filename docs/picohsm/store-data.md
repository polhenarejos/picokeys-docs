# Store data

Pico HSM can also store non-key data objects. This is useful, but it should be kept in proportion: the device is a secure cryptographic appliance, not a general object store.

## Typical use cases

Reasonable uses include:

- certificates
- small metadata blobs
- application-bound configuration
- auxiliary secrets tied to the same access-control model

Large arbitrary file storage is not the intended design target.

## Write a data object

```sh
echo "This is a test data" > data.bin

pkcs11-tool \
  --write-object data.bin \
  --type data \
  --id 30 \
  --label my-data \
  --pin 648219
```

## List data objects

```sh
pkcs11-tool \
  --list-objects \
  --type data \
  --pin 648219
```

## Read a data object

```sh
pkcs11-tool \
  --read-object \
  --type data \
  --id 30 \
  --output-file read-data.bin \
  --pin 648219
```

## Delete a data object

```sh
pkcs11-tool \
  --delete-object \
  --type data \
  --id 30 \
  --pin 648219
```

## Practical caution

Data objects inherit the management complexity of the HSM:

- you still need the right session and PIN context
- object identifiers need discipline
- restore and migration expectations should be documented up front

Use them deliberately, not as a dumping ground.
