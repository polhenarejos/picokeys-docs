# Anti-rollback

Secure boot blocks unsigned firmware. Anti-rollback covers a separate case: an old image that is still correctly signed.

If an older signed image contains a serious vulnerability, secure boot alone may still allow it to run. Anti-rollback rejects firmware below a configured version floor.

## Why it exists

The downgrade can happen in a few steps:

1. a device owner signs firmware version A
2. version A later turns out to have a security bug
3. version B fixes it
4. an attacker with physical access flashes old signed version A

Anti-rollback closes that path when implemented and enabled correctly.

## Irreversibility

On RP2350-class hardware, rollback state is normally stored in one-time-programmable memory. As a result:

- floors go up, not down
- rollback budget is finite
- mistakes can orphan older images permanently

Use this production feature only after you have planned for those permanent limits.

## Applicability

Use these questions to guide verification; commands from another project may not apply to Pico FIDO or Pico OpenPGP.

For PicoKeys firmware, verify:

- board family
- secure boot status
- available provisioning tool
- image signing flow
- documented rollback behavior

## Operational rule

Raise the rollback floor only when you have a clear reason, such as:

- a security fix blocks downgrade exploitation
- old signed images should no longer be accepted
- the team accepts the irreversible state change

Do not spend rollback budget on routine releases unless your process explicitly requires it.
