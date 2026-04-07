# Operational limits

This page captures practical limits and expectations for Pico OpenPGP operations.

---

## Performance profile

Operation time depends on algorithm and key size.

In general:

- ECC operations are faster
- Larger RSA keys increase latency significantly

Plan UX and automation timeouts accordingly, especially for high-bit RSA workflows.

---

## Tooling limitations

Not every client tool exposes every card capability.

For some advanced operations, support may depend on:

- specific middleware versions
- PKCS#11 module behavior
- direct APDU tooling

UI availability is not always equivalent to firmware capability availability.

If a feature appears missing in one client, verify with an alternative tool path before concluding firmware regression.

---

## Environment dependencies

Runtime behavior is affected by:

- host OS smart-card stack
- PC/SC service state
- USB reader/device recognition
- board/firmware configuration choices

Treat these dependencies as part of deployment validation, not as optional post-setup checks.

---

## Capacity and workflow planning

For reliable operations in production-like environments:

- Prefer ECC when latency matters.
- Reserve large RSA key operations for explicit cases that require them.
- Budget time for unlock/retry/recovery flows in automation pipelines.
- Separate “fast path” daily operations from “maintenance path” admin operations.

---

## Troubleshooting priority order

When operations fail or are unexpectedly slow, use this order:

1. Confirm device/session state (connected, unlocked, correct role context).
2. Confirm host middleware health (PC/SC, reader visibility).
3. Re-test with a second client/tooling path.
4. Then inspect firmware-specific behavior.

This ordering reduces false positives and shortens incident triage time.
