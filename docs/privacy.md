# Privacy — what ZAC reads, sends, and stores

This is the most important page in this repository. ZAC runs on your PC and
inspects it, so you deserve a precise, honest answer to one question: **what
happens to my data?** Here it is, in full.

> **The short version.** ZAC's central service stores **nothing that identifies
> you**. Your PC scan never leaves your machine in raw form — only a clean /
> suspicious *verdict* does, and it goes **straight to the game server you
> chose to join**, not to us. ZAC installs **no kernel driver** and reads
> **no** chat, files, browser data, or Discord tokens.

## At a glance

| | **Reads it?** | **Leaves your PC?** | **Stored by ZAC centrally?** |
|---|---|---|---|
| Hardware / OS signals for the scan | Yes, locally | No (only a verdict does) | **No** |
| A per-machine TPM key | Created locally | Public part only, at enroll | **No identity** — see below |
| Clean / suspicious **verdict** | Produced locally | Yes → to the **game server** | **No** |
| Your name, Steam ID, IP, email | **No** | No | **No** |
| Chat, documents, browser history | **No** | No | **No** |
| Discord token / messages | **No — forbidden** | No | **No** |

## The two parties, and who holds what

ZAC splits responsibility on purpose, so that the party with no need for your
identity never receives it.

### ZAC Central (us) — stores no personal data

Our central service is almost static. It publishes:

- ZAC's **root public key** (so servers can verify clients offline),
- a **revocation list** (fingerprints of revoked/abused client certificates),
- the current **client version** and update files.

That is all. We hold **no** Steam IDs, names, IPs, machine profiles, per-player
reputation, or cross-server history. We are never in the path while you play.
When the client enrolls, we verify your machine's security-chip attestation and
then **discard** the hardware identity — we keep only a yes/no marker that the
key is hardware-backed, never anything that points back to your specific PC.

### The game server you join (the "tenant") — controls its own records

The server owner already knows who you are — FiveM hands them your Steam,
Discord, and license identifiers the moment you connect, with or without ZAC.
The owner may store, in **their own** database under **their own** control:

- a link between those identifiers and a **salted hardware hash** (so a banned
  cheater can't dodge by making a new Steam account), and
- detections and their own ban list.

The hardware hash is salted **per server**, so the same PC looks different to
every server — owners cannot pool a global profile of you. This is the owner's
data about their own players, exactly as it was before ZAC existed. ZAC never
sees it.

## What the verdict contains

When you join a ZAC server, the client sends that server a signed message
containing essentially: *"a genuine, untampered ZAC client is live on this
machine right now, and its scan result is **clean** (or **suspicious**), at this
confidence."* It does **not** contain the raw findings of the scan — not a list
of your processes, not your files, not your hardware serials. A verdict class
and a confidence number, nothing more.

## What ZAC will never do

- **No kernel driver / rootkit.** User-mode only.
- **No reading of Discord tokens or messages.** Reading the Discord token off
  disk is malware behaviour; ZAC forbids it outright. (If a server links your
  Discord, it does so from the identifier FiveM already gives it — not from us.)
- **No chat, document, browser, or email access.**
- **No selling or sharing of data.** We have no personal data to sell.
- **No silent background data collection while you're not playing.**

## Your rights (GDPR)

Because ZAC Central holds no personal data about you, there is nothing for us to
expose, sell, or leak. Any personal data lives with the **server owner**, who is
the data controller for their own players (as they already were). Requests to
access or delete your data are directed to the server you played on; we publish
a [data-processing summary](#) <!-- TODO: link DPA / privacy whitepaper --> to
help owners meet those obligations.

## Don't take our word for it

- The client is **user-mode** — Task Manager shows it; you can stop it anytime.
- Network traffic is inspectable: you'll see the client talk to the **game
  server** and, occasionally, to ZAC for version/revocation updates — never a
  stream of personal data.
- Every release is published with a **VirusTotal** link and a checksum (see
  [Downloads](../README.md#downloads)).

If you find ZAC doing anything this page does not describe, that is a security
issue — please [report it](../SECURITY.md). We mean this literally.
