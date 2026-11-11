# PIV

PIV is a smart-card application model for X.509 certificates, client authentication, smart-card login, PKCS#11, and related workflows.

Before following these steps on a standalone Pico OpenPGP device, check that its firmware image includes the PIV applet.

## Applicability check

Before provisioning, confirm the applet exists:

```sh
ykman piv info
```

or use OpenSC/PCSC tooling to inspect available applications.

If the command cannot find a PIV applet, the instructions below do not apply to that firmware image.

## Defaults and provisioning

PIV deployments normally include:

- PIN
- PUK
- management key
- key slots such as `9a`, `9c`, `9d`, and `9e`

Change the published default credentials before production use.

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

The PKCS#11 module path varies by operating system.

## Caveats

- `ykman piv` may depend on YubiKey-like reader naming.
- PIV support is not the same thing as OpenPGP support.
- Imported keys and generated keys have different attestation and recovery properties.

PIV has a separate applet and lifecycle, so manage it independently from OpenPGP.

## Current policy defaults

The current firmware reports slot metadata for PIN and touch policy, origin,
and algorithm. Unless explicitly overridden, the defaults are:

- signature slot: PIN required for every use;
- card-authentication slot: PIN-free use;
- other key slots: PIN required once per authenticated session; and
- generated/imported keys: no touch requirement.

Review the reported slot metadata before relying on these defaults. Set an
explicit policy when your deployment requires touch or a different PIN
boundary.

The PIV attestation key and its metadata are exposed through slot `F9`. Retired
keys can also be moved back to active slots in current firmware, so use the
move operation deliberately and verify the destination slot before committing.
