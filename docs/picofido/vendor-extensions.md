# Vendor extensions

Pico FIDO includes vendor extensions beyond baseline FIDO interoperability.

These features can be useful, but they are less portable than baseline FIDO behavior and should be documented accordingly.

## User PIN split

The current PicoKeys docs describe a dual-context model:

- Admin PIN for full management
- User PIN for a restricted subset of actions

This product-specific management model does not carry over to ordinary WebAuthn clients.

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

Permission masks make Pico FIDO more like a policy-managed authenticator, so clients may behave differently from how they do with a standard FIDO key.

## Extended credential views

PicoKey App also provides management operations such as:

- exporting metadata as JSON or CSV
- listing creation dates, algorithms, counters, and `credProtect` state
- filtering by RP, user, or credential identifier
- per-credential Large Blob handling

These management features are useful within the PicoKey App, but other platforms are not guaranteed to expose them.

## How to think about these features

Use vendor extensions when:

- you control the operational environment
- you need the added policy surface
- you can rely on PicoKey-specific tooling

Do not build a portability story around them unless you have tested the exact host tools you intend to support.

## Conservative rule

Test interoperability explicitly before promising support for any feature outside the baseline CTAP/WebAuthn path.
