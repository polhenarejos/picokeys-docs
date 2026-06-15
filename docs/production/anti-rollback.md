# Anti-rollback

Secure boot prevents unsigned firmware from running. Anti-rollback addresses a different problem: old firmware that is still correctly signed.

If an older signed image contains a serious vulnerability, secure boot alone may still allow it. Anti-rollback is the mechanism that refuses firmware below a configured floor.

## Why it exists

The downgrade threat is simple:

1. a device owner signs firmware version A
2. version A later turns out to have a security bug
3. version B fixes it
4. an attacker with physical access flashes old signed version A

Anti-rollback closes that path when implemented and enabled correctly.

## Irreversibility

On RP2350-class hardware, rollback state is normally backed by one-time-programmable state. That means:

- floors go up, not down
- rollback budget is finite
- mistakes can orphan older images permanently

This is a production feature, not a day-one convenience setting.

## Applicability

Use this page as the model. Do not assume the commands from another project apply directly to Pico FIDO or Pico OpenPGP.

For PicoKeys firmware, verify:

- board family
- secure boot status
- available provisioning tool
- image signing flow
- documented rollback behavior

## Operational rule

Raise a rollback floor only for a reason you can name:

- a security fix blocks downgrade exploitation
- old signed images should no longer be accepted
- the team accepts the irreversible state change

Do not spend rollback budget on routine releases unless your process explicitly requires it.
