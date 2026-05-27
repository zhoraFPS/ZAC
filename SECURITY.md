# Security Policy

We take security seriously and we welcome good-faith research. ZAC's whole value
is trust, and trust is strengthened — not threatened — by responsible
disclosure.

## Reporting a vulnerability

**Please do not open a public issue for security problems.**

Email **security@z-hub.app** <!-- TODO: confirm security contact address --> with:

- a description of the issue and its impact,
- steps to reproduce (proof-of-concept welcome),
- your assessment of severity, and
- how you'd like to be credited (or to stay anonymous).

We aim to acknowledge reports within **72 hours** and to keep you updated as we
investigate and fix.

## What we consider in scope

- Ways to bypass attestation, forge a "clean" verdict, or relay/replay one.
- Privacy issues: any path by which personal data reaches the central service,
  or by which the client reads or transmits more than this repository documents.
- Integrity issues in the signed-update / download-verification chain.
- Server-side resource issues (the `kernel_ac` integration) that let an
  unauthenticated party deny service or bypass the connect gate.

## What is *not* a vulnerability

- The fact that a determined attacker who fully owns their machine can, in
  principle, run an external-hardware (DMA / capture-card) cheat. This is a
  named, documented residual — see
  [security-model.md](docs/security-model.md#honest-residuals). It is mitigated
  and detected, never claimed to be eliminated.
- Reverse-engineering the client. Our security does not depend on the client's
  secrecy (see Kerckhoffs's principle in the security model). Reports that turn
  RE into a *concrete bypass*, however, are very much in scope.

## Safe harbour

Good-faith research that respects user privacy, avoids service disruption, and
follows this policy will not be pursued legally. We will work with you and
credit your finding (with your consent).
