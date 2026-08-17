# PIV

PIV is a smart-card application model for X.509 certificates, client authentication, smart-card login, PKCS#11, and related workflows.

This page exists because the `RS-Key` documentation has a PIV section and PicoKeys tooling has PIV-oriented workflows. For a standalone Pico OpenPGP deployment, verify that your firmware image actually includes the PIV applet before treating any command here as applicable.

## Applicability check

Before provisioning, confirm the applet exists:

```sh
ykman piv info
```

or use OpenSC/PCSC tooling to inspect available applications.

If the command cannot find a PIV applet, this page is not applicable to that firmware image.

## Defaults and provisioning

PIV deployments normally include:

- PIN
- PUK
- management key
- key slots such as `9a`, `9c`, `9d`, and `9e`

Change defaults before real use. Public defaults are provisioning state, not production state.

## Generate a key

Typical Yubico-style tooling uses:

```sh
ykman piv keys generate --algorithm ECCP256 9a pub.pem
ykman piv certificates generate --subject "CN=me" 9a pub.pem
```

For a CA-issued certificate, generate a CSR and import the issued certificate instead of using a self-signed certificate.

## Use through PKCS#11

Many applications can use a PIV token through OpenSC:

```sh
ssh-keygen -D /usr/lib/opensc-pkcs11.so
ssh -I /usr/lib/opensc-pkcs11.so you@host
```

The exact module path depends on the operating system.

## Caveats

- `ykman piv` may depend on YubiKey-like reader naming.
- PIV support is not the same thing as OpenPGP support.
- Imported keys and generated keys have different attestation and recovery properties.

Treat PIV as its own applet with its own lifecycle.

## Current policy defaults

The current firmware reports slot metadata for PIN and touch policy, origin,
and algorithm. Unless explicitly overridden, the defaults are:

- signature slot: PIN required for every use;
- card-authentication slot: PIN-free use;
- other key slots: PIN required once per authenticated session; and
- generated/imported keys: no touch requirement.

These defaults are not a substitute for reviewing slot metadata. Set an
explicit policy when your deployment requires touch or a different PIN
boundary.

The PIV attestation key and its metadata are exposed through slot `F9`. Retired
keys can also be moved back to active slots in current firmware, so use the
move operation deliberately and verify the destination slot before committing.
