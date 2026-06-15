# PINs and roles

Pico OpenPGP follows the usual OpenPGP card split between user and admin credentials. If you ignore that split, many later failures look mysterious when they are not.

## The three domains

The important credentials are:

| Domain | Typical role |
| --- | --- |
| `PW1` | user operations |
| `PW3` | administrative operations |
| `RC` | user-PIN recovery or reset path |

The exact UI differs by tool, but the role boundary does not.

## `PW1`

This is the day-to-day user context:

- signing
- decryption
- authentication
- normal use of already-provisioned material

It is the credential most end users should know.

## `PW3`

This is the administrative context:

- key generation and import
- policy changes
- sensitive card settings
- destructive or recovery-sensitive actions

If an operation changes card state rather than just using it, assume `PW3` is involved until proven otherwise.

## `RC`

The reset code is a recovery credential. Treat it accordingly.

If you keep it equal to an easily known admin secret or fail to document who holds it, the recovery model is weak even if the firmware supports it correctly.

## Retry counters and lockouts

This is where many users get surprised:

- each PIN domain has its own retry logic
- a wrong attempt decrements the relevant counter
- a correct entry resets that counter
- a blocked admin path is far more painful than a blocked user path

That is not Pico-specific. It is still worth stating explicitly because it defines the recovery runbook.

## Practical rule

Before any sensitive operation, know which context you are in:

1. read-only status does not prove admin access
2. a user session does not imply an admin session
3. reconnects can clear the context you assumed was still active

Most "the card refused the command" reports reduce to one of those.
