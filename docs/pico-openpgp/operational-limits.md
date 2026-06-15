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

Those numbers are not cosmetic. They determine whether RSA is practical for your workflow at all.

## ECC is usually the sensible default

For most day-to-day OpenPGP use:

- ECC generation is much faster
- interactive workflows feel saner
- automation is less likely to hit timeouts or impatient users

If you do not have a specific RSA requirement, do not make large RSA your default choice.

## Firmware support is not client support

This is the recurring mistake in OpenPGP docs.

Examples:

- AES may exist in firmware but not be easy to use in mainstream OpenPGP tools
- advanced card functions may require lower-level tooling
- GnuPG, OpenSC, and PKCS#11 clients do not surface the same feature set

So "supported by Pico OpenPGP" must always be qualified by the client path.

## Practical rule

Before you depend on a feature, verify all three:

1. the firmware implements it
2. the host middleware passes it through correctly
3. the client you intend to use exposes it in a stable way

If one of those fails, the feature is not operationally available for your deployment, whatever the firmware README says.
