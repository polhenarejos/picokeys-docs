# Store arbitrary data

This document explains how to store and retrieve arbitrary data objects in Pico HSM, based directly on the official Pico HSM documentation.

This feature allows using the device as a secure data store for small binary blobs protected by the HSM access control model.

## Overview

Pico HSM allows storing arbitrary binary data as objects.

Stored data:

- Is kept inside the device
- Is protected by access control
- Can be listed, read, and deleted

!!! note
    Stored data is not cryptographic key material, but it is protected by the same secure storage mechanisms.

## Store data

Create a file containing the data to be stored:

```bash
echo "This is a test data" > data.bin
```

Store the data in Pico HSM:

```bash
pkcs11-tool \
  --write-object data.bin \
  --type data \
  --id 30 \
  --label my-data \
  --pin 648219
```

The data is now stored inside the device.

## List stored data objects

To list stored data objects:

```bash
pkcs11-tool \
  --list-objects \
  --type data \
  --pin 648219
```

This command shows all stored data objects with their identifiers and labels.

## Read stored data

To read a stored data object:

```bash
pkcs11-tool \
  --read-object \
  --type data \
  --id 30 \
  --output-file read-data.bin \
  --pin 648219
```

The content of read-data.bin will match the original stored data.

## Delete stored data

To delete a stored data object:

```bash
pkcs11-tool \
  --delete-object \
  --type data \
  --id 30 \
  --pin 648219
```

!!! warning
    Deleting a data object is irreversible.

## Data size limitations

Stored data objects are subject to size limitations.

!!! note
    Pico HSM is designed for small binary blobs, not large file storage.

## Use cases

Typical use cases for stored data include:

- Configuration blobs
- Certificates
- Metadata
- Application-specific secrets

!!! tip
    Use stored data for information that must remain bound to the device.

## Security considerations

When storing arbitrary data:

- Use strong access control
- Avoid storing sensitive data unnecessarily
- Delete unused data objects

!!! warning
    Stored data is protected, but not encrypted per-object like private keys.

---

## Summary

The data storage feature in Pico HSM allows:

- Secure storage of small binary data
- Controlled access via PKCS#11
- Simple read/write/delete operations

This makes Pico HSM suitable for storing metadata and auxiliary information alongside cryptographic keys.
