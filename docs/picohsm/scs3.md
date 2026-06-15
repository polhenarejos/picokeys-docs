# Smart Card Shell 3

Smart Card Shell 3 (SCS3) matters for Pico HSM because some advanced workflows are not well covered by generic PKCS#11 tooling.

That is the honest reason to use it.

## When SCS3 becomes relevant

According to upstream documentation, SCS3 is the path for workflows such as:

- importing PKCS#12 private keys and certificates
- importing WKY-wrapped material
- operating with CardContact-style SC-HSM logic that generic CLI tools do not expose cleanly

If you only need ordinary key generation and signing, you probably do not need SCS3 yet.

## Why setup is awkward

The upstream instructions explain that SCS3 trusts CardContact manufacturing certificates by default, so Pico HSM users need to extend the trust store for Pico HSM's CA material before advanced operations work.

That is not a bug in your setup. It is part of adapting a CardContact-oriented toolchain to a different device family.

## What to expect

Using SCS3 successfully usually means:

- editing trust-store configuration
- understanding the SC-HSM JavaScript support files
- authenticating correctly before import/export actions
- having the required DKEK context available for wrapped-key operations

This is advanced-admin territory, not a beginner workflow.

## Operational advice

- keep a tested copy of your SCS3 trust-store changes under version control
- document the exact SCS3 version used
- test imports on disposable material first
- do not discover DKEK or trust-anchor problems during a recovery event

That last point is the whole reason to document SCS3 carefully.
