# Smart Card Shell 3

Smart Card Shell 3 (SCS3) is useful with Pico HSM when a workflow is not well covered by generic PKCS#11 tools.

## When SCS3 becomes relevant

According to upstream documentation, SCS3 is the path for workflows such as:

- importing PKCS#12 private keys and certificates
- importing WKY-wrapped material
- operating with CardContact-style SC-HSM logic that generic CLI tools do not expose cleanly

For ordinary key generation and signing, you can usually work without SCS3.

## Why setup is awkward

The upstream instructions explain that SCS3 trusts CardContact manufacturing certificates by default, so Pico HSM users need to extend the trust store for Pico HSM's CA material before advanced operations work.

This trust-store change adapts a CardContact-oriented toolchain to Pico HSM; it does not indicate a setup failure.

## What to expect

Using SCS3 successfully usually means:

- editing trust-store configuration
- understanding the SC-HSM JavaScript support files
- authenticating correctly before import/export actions
- having the required DKEK context available for wrapped-key operations

These steps are intended for administrators who already understand the toolchain.

## Operational advice

- keep a tested copy of your SCS3 trust-store changes under version control
- document the exact SCS3 version used
- test imports on disposable material first
- do not discover DKEK or trust-anchor problems during a recovery event

Documenting these details now can prevent trust-anchor or DKEK problems during recovery.
