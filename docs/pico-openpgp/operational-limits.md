# Operational limits

Pico OpenPGP has two classes of limits:

- performance limits
- client and middleware limits

Both matter in practice.

## RSA key generation is expensive

The upstream README publishes indicative timings that materially affect expectations:

| RSA key length | Key generation | Signature / decrypt |
| --- | ---: | ---: |
| 1024 bits | about 16 s | about 1 s |
| 2048 bits | about 124 s | about 3 s |
| 3072 bits | about 600 s | about 7 s |
| 4096 bits | about 1000 s | about 15 s |

These timings help determine whether RSA fits your workflow, especially when key generation is part of regular provisioning.

## ECC is usually the sensible default

For everyday OpenPGP use, ECC is often more practical because:

- ECC generation is much faster
- interactive workflows feel saner
- automation is less likely to hit timeouts or impatient users

Unless you have a specific RSA requirement, prefer ECC for new deployments.

## Firmware support is not client support

Keep firmware support and client support separate when you evaluate a feature.

Examples:

- AES may exist in firmware but not be easy to use in mainstream OpenPGP tools
- advanced card functions may require lower-level tooling
- GnuPG, OpenSC, and PKCS#11 clients do not surface the same feature set

When you say that Pico OpenPGP supports a feature, name the client path you have tested as well.

## Practical rule

Before you depend on a feature, verify all three:

1. the firmware implements it
2. the host middleware passes it through correctly
3. the client you intend to use exposes it in a stable way

If one of those fails, the feature is not operationally available for your deployment, whatever the firmware README says.
