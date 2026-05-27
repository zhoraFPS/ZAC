# Disclosure Policy — what this repository documents, and what it never will

ZAC is closed-source, but we publish everything needed to *trust* it. This
document is the rule we apply to decide what goes in this public repository. It
is public on purpose: a disciplined, stated boundary is itself a trust signal.

## The one rule

> **We disclose the trust model. We never disclose the arms-race layer.**
>
> If a sentence helps an honest player, owner, or security researcher
> understand *that* something is protected and *why* — it belongs here.
>
> If a sentence would hand a cheater a concrete recipe to evade detection —
> it stays out.

## Always public (the trust model)

These rest on cryptography and design, not secrecy. Publishing them does not
weaken ZAC (see [security-model.md](docs/security-model.md) — Kerckhoffs's
principle), and it directly earns trust:

- The overall architecture: the "key & lock" model, the two-layer defence.
- Cryptographic **principles**: signature scheme family, TPM-backed key
  protection, the central root + revocation list, single-use challenge nonces,
  offline verifiability.
- All **data flows**: what the client reads locally, what leaves the machine,
  what is stored where, and under whose control.
- The full **privacy / GDPR** posture.
- The **categories** of cheating ZAC detects.
- **Honest limits**: the residual risks no client-side anti-cheat can eliminate.
- Everything a **server owner** needs to install, configure, and operate ZAC.

## Never public (the arms-race layer)

These are the only things whose secrecy actually matters. They give no
legitimate user any benefit, and give cheaters an evasion advantage:

- Specific detector **signatures, heuristics, or thresholds**.
- The internal **mechanism** of any individual detector (the *how*, not the
  *what*).
- **Honeypot** designs and decoy behaviour.
- Anti-tamper, anti-debug, and **polymorphic build** internals.
- Internal cheat **research notes** and exploit write-ups.
- Source code, build pipelines, signing keys, internal endpoints, schemas of
  the detection engine.

## Enforcement

A CI check ([`leak-guard`](.github/workflows/leak-guard.yml)) scans every
change for terms that signal an accidental leak (internal detector names,
research-note filenames, threshold-shaped content) and fails the build. It is a
safety net, not a substitute for judgement: **when in doubt, leave it out.**
