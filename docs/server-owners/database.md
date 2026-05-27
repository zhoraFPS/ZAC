# Data & storage (server owners)

This page explains what data ZAC involves on the server side, where it lives, and
who controls it. The short answer: **the data about your players is yours, under
your control** — the same identifiers FiveM already gives you.

## What the resource itself stores

The `kernel_ac` resource keeps **no disk or database state of its own**. Its only
state is an in-memory table of who has been verified in the current session. When
a player connects it:

- reads their `steam:`, `license:` (or `license2:`), and `discord:` identifiers
  from FiveM,
- and uses them to confirm — via the ZAC backend — that a healthy, non-banned
  ZAC client belongs to that player.

The plaintext identifiers **never leave your FXServer**; only hashed digests are
used for the lookup.

## Who controls what

| Data | Where it lives | Controller |
|---|---|---|
| Your players' identifiers (Steam / Discord / license) | Already on your FXServer (FiveM provides them) | **You** |
| Your detections and ban list | Your tenant records | **You** |
| ZAC root key, revocation list, client version | ZAC Central | ZAC — **no player data** |

ZAC Central is the controller of **no player personal data**. You are the
controller of your own players' data, exactly as you were before installing ZAC.
This split is the core of ZAC's [privacy posture](../privacy.md) and its GDPR
story.

## Bans and ban evasion

When you ban a player, linking their identifiers to a **salted hardware hash**
lets a hardware ban survive a fresh Steam or Discord account — the cheater has to
change actual hardware to dodge it. The hash is salted **per tenant**, so the same
machine looks different to every server; no global cross-server profile is built.

> **Hardware bans carry false-positive risk** (shared PCs, hardware swaps). Use
> them alongside a verdict and your own judgement, not blindly.

## Roadmap note

ZAC's attestation architecture is moving identity-linkage and detection storage
fully **tenant-side** (into your own datastore — `oxmysql`, with a FiveM-KVP
fallback for tiny servers), so that **no player-identifying data reaches ZAC
Central at all**. This page will be updated with the exact schema and convars as
that lands. The data-control principle above is the destination: your players'
data stays with you.
