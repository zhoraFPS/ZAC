# How ZAC works

ZAC protects a FiveM server with **two cooperating layers**. This page explains
both in plain language. It deliberately covers *what* happens and *why* — not
the internal *how* of detection (see the [disclosure policy](../DISCLOSURE-POLICY.md)).

## The key & lock model

Think of a ZAC-protected server as a locked door.

- The **ZAC client** on your PC is the **key**. It is not just a password — it
  is a key that also inspects your machine and proves, cryptographically, that
  no cheat is present on it.
- The **server resource** is the **lock**. It checks that the key is genuine and
  decides whether to let you in.

A server running ZAC's server side **and** requiring the ZAC client is a locked
door that only a clean machine running a genuine client can open. No client →
the door stays shut, with a friendly message telling you where to download it.

## Layer 1 — Client attestation (the universal key)

This layer proves *a genuine, untampered ZAC client is live on your machine, and
reports the result of its local scan.*

1. **Enroll once.** The first time the client runs, it generates a private key
   **inside your computer's TPM** (the security chip Windows 11 already
   requires). That key can never be copied off your machine. The client gets a
   certificate that says "this is a genuine ZAC client" — signed by ZAC's root
   key. No account, no login, no personal data.
2. **Join a server.** When FiveM connects to a ZAC server, the client notices
   and sends a **signed attestation directly to that server** over HTTPS. The
   server replies with a one-time random challenge ("nonce"); the client signs
   it. This proves the client is *live right now* — an old recording can't be
   replayed.
3. **The verdict travels, not your data.** The attestation carries a
   **verdict** (clean / suspicious) and a confidence level — *not* the raw
   details of what was scanned. (More in [Privacy](privacy.md).)
4. **The server decides.** It verifies the signature against ZAC's bundled root
   key and revocation list — **fully offline**, ZAC's servers are never in the
   loop — then applies its own policy: let you in, or kick.

The trust material travels on a **direct, signed channel** between the client
and the server. It never passes through the game's in-browser UI, which can be
tampered with — so tampering there gains an attacker nothing.

### While you play

The server re-issues a fresh challenge every so often. If you close the ZAC
client mid-session, the next challenge goes unanswered and you're dropped within
one interval. You cannot "connect clean, then turn the client off."

## Layer 2 — Server-authoritative detection (the lock's own senses)

A client can, in theory, be faked. So the server **also** watches the game state
itself — things the client cannot lie about: impossible movement, health and
weapon anomalies, economy exploits, injected resources, abnormal event rates.

This is defence-in-depth: a faked or lying Layer-1 client is still caught by
Layer 2, and cheats that live entirely off your PC (which Layer 1's scan can't
see) are caught by Layer 1. Neither alone is enough; together they are strong.

## Zero friction, by design

- **Players:** install once, then every ZAC server "just works." Roaming between
  protected servers needs zero re-configuration.
- **Owners:** drop in one resource, set a few convars, done. No infrastructure
  to run — see the [owner guide](server-owners/install.md).

## What this design deliberately avoids

- **No kernel driver.** ZAC runs in user-mode. Lower risk to your system, and
  nothing that can brick a boot.
- **No central data hoard.** ZAC's central service is almost static — a
  publisher of keys and version info — and holds nothing that identifies you.
  See [Privacy](privacy.md) and the [Security model](security-model.md).
