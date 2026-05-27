# Security model

This page states, honestly, what ZAC defends against, what it cannot, and why we
can document all of this in public without helping cheaters.

## Our threat model

We start from the only honest baseline for client-side anti-cheat:

> **Assume the attacker fully owns their machine.**

A determined attacker *can*: run as administrator; load kernel code via a
vulnerable driver; attach a debugger and patch files on disk; intercept their
own network traffic; reimplement our protocol (write a "fake client"); use a
**second PC or external hardware** (a DMA card, or a capture-card + hardware
mouse aimbot); partially spoof hardware IDs; disable their TPM; run in a virtual
machine.

A determined attacker *cannot*: break modern cryptography (ECDSA / TLS);
extract a non-exportable key from a TPM; or forge a signature from ZAC's root
key.

Everything below follows from that line.

## Trust boundaries

| Party | How much we trust it | What it holds |
|---|---|---|
| **ZAC Central** | Trusted publisher | Root key, revocation list, version manifest — **no player data**. |
| **Game server (tenant)** | Trusted controller of *its own* players | Identity linkage, detections, ban list — runs the lock and its own server-side detection. |
| **The client agent** | **Semi-trusted** | Best-effort tamper-resistant; **never fully trusted** — backstopped by server-side detection. |

The key design decision: a presence-and-scan attestation is **necessary but not
sufficient.** A faked client with a genuine key could sign "clean" without
really scanning. That is exactly why ZAC has a second, server-authoritative
layer — see [How ZAC works](how-it-works.md#layer-2--server-authoritative-detection-the-locks-own-senses).

## The cryptographic foundation

- **Per-machine key in the TPM.** Generated inside the hardware security chip,
  non-exportable. Signing happens in the chip; the private key never exists in
  ordinary memory. Stealing it becomes a hardware problem, not a software one —
  so there is no universal, sellable "clean key" to leak.
- **Hardware-backed tier is unforgeable.** At enrollment the key's
  hardware-backing is attested and baked into the ZAC-signed certificate.
  A player who disables their TPM to fall back to a weaker software key gets a
  visibly *software-tier* certificate — the downgrade is **visible, not
  silent** — and servers can (and by default do) require hardware-backed keys.
- **Single-use challenges.** Each attestation answers a fresh, server-issued
  random nonce, judged by **server time**. Old attestations can't be replayed.
- **Offline verification.** Servers verify against a **bundled** root key and a
  cached revocation list. ZAC Central being down never breaks verification.
- **Revocation.** An abused certificate's fingerprint can be published to the
  revocation list; servers pick it up within their refresh window. No personal
  data is involved.

## Honest residuals

No client-side anti-cheat — not EAC, BattlEye, or Vanguard — eliminates these.
We name them rather than hide them:

1. **A local attacker asking their own TPM to sign while cheating.** The key is
   genuine; the lie is in the verdict. This is the job of the detection
   arms-race (Layer 1's scan + Layer 2's server-side behaviour), and it is never
   "solved" — only continuously raised.
2. **DMA / external-PC aimbots.** Cheats that run on separate hardware, feeding
   input through a device that looks like a real mouse. ZAC actively **detects**
   categories of these (see [what we detect](what-we-detect.md)) and Layer 2
   catches the resulting behaviour — but no software running on the victim PC can
   *eliminate* a cheat that doesn't live on that PC.

We would rather you trust a true claim than be impressed by a false one.

## Why closed source, but transparent

ZAC's source is closed, yet we publish the entire trust model above. These are
not in tension — they follow **Kerckhoffs's principle**:

> A system should be secure even if everything about it, except the key, is
> public knowledge.

ZAC's security rests on **secrets that stay secret no matter what we publish**:
the TPM-bound private keys (which never leave hardware) and ZAC's root signing
key. It does **not** rest on hiding the protocol. Our own threat model already
assumes the attacker reverse-engineers the client — so describing the
architecture in public costs us nothing.

What *would* help a cheater is the **arms-race layer** — the specific detection
signatures, thresholds, and anti-tamper internals. Those, and only those, we
keep closed. The boundary is written down and enforced; see the
[Disclosure Policy](../DISCLOSURE-POLICY.md).

The result: you can fully evaluate *whether to trust ZAC* — its data handling,
its cryptography, its honest limits — without us exposing anything that makes
the next cheat easier to write.
