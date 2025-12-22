First steps

This document provides a general usage walkthrough for Pico HSM, based directly on the official Pico HSM documentation.

It shows how to interact with the device using standard command-line tools.

## Overview

Pico HSM behaves as a smart-card-compatible device.

Interaction is typically performed using:

- pkcs11-tool
- OpenSSL
- OpenSC utilities

!!! note
    Pico HSM does not require proprietary tools for normal operation.

## Detect the device

List available readers:

```bash
opensc-tool -l
```

List cards and applications:

```bash
opensc-tool -a
```

If Pico HSM is detected correctly, it will appear as a smart card application.

## List objects

List all objects stored on the device:

```bash
pkcs11-tool --list-objects --pin 648219
```

This includes:

- Private keys
- Public keys
- Secret keys
- Stored data objects
