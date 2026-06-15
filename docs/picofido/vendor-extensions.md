# Vendor extensions

This page covers Pico FIDO behavior that goes beyond baseline FIDO interoperability.

That does not make these features bad. It means they should be documented honestly: useful, powerful, and less portable.

## User PIN split

The current PicoKeys docs describe a dual-context model:

- Admin PIN for full management
- User PIN for a restricted subset of actions

That is not part of normal WebAuthn portability. It is a product-specific management model.

If you enable it, write down:

- who holds the Admin PIN
- what the User PIN is allowed to do
- how operators recover when the wrong context is active

## Permission-scoped management

The documented permission mask includes capabilities such as:

- creating credentials
- credential management
- authentication
- Large Blob write
- authenticator configuration
- reset and persistent management actions

This is powerful because it turns Pico FIDO into something closer to a policy-managed authenticator. It is also where assumptions about "works like any other FIDO key" stop being true.

## Extended credential views

The current docs also describe app-level operations such as:

- exporting metadata as JSON or CSV
- listing creation dates, algorithms, counters, and `credProtect` state
- filtering by RP, user, or credential identifier
- per-credential Large Blob handling

These are management conveniences, not guaranteed cross-platform behaviors.

## How to think about these features

Use vendor extensions when:

- you control the operational environment
- you need the added policy surface
- you can rely on PicoKey-specific tooling

Do not build a portability story around them unless you have tested the exact host tools you intend to support.

## Conservative rule

If a feature is not part of the baseline CTAP/WebAuthn path, assume it needs explicit interoperability testing before you promise it to anyone else.
