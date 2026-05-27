# ZAC Client

**ZAC is a two-layer anti-cheat for FiveM.** A lightweight client proves your PC
is clean, and the server independently watches gameplay it cannot be lied to
about. Neither layer alone is enough; together they are enterprise-grade.

This repository is the **public face of ZAC**: how it works, exactly what it
does (and does not do) on your machine, how server owners integrate it, and
where to download the official, signed client.

> **The source code of the ZAC client and detection engine is closed.** That is
> a deliberate choice, not a contradiction with transparency — see
> [Why closed source, but open about how it works](docs/security-model.md#why-closed-source-but-transparent).
> Everything that matters for *trust* is documented here in full.

---

## Three things to know up front

- 🛡️ **User-mode. No kernel driver.** ZAC does **not** install a ring-0 / kernel
  driver. It runs as an ordinary Windows program you can see, stop, and remove.
- 🔒 **Privacy by design.** ZAC's central service stores **nothing that
  identifies you** — no Steam ID, no name, no IP, no machine profile, no
  cross-server history. [See exactly what runs on your machine →](docs/privacy.md)
- 🔑 **The client is your key.** Install once. Joining any ZAC-protected server
  then "just works" — no accounts, no tokens, no per-server setup.

---

## For players

| Doc | What it answers |
|-----|-----------------|
| [How ZAC works](docs/how-it-works.md) | The "key & lock" model, in plain language. |
| [Privacy](docs/privacy.md) ⭐ | What ZAC reads, what leaves your PC, what is stored, and where. |
| [What ZAC detects](docs/what-we-detect.md) | The categories of cheating ZAC targets — and an honest note on limits. |
| [Security model](docs/security-model.md) | Our threat model and why we can be this open without helping cheaters. |
| [FAQ](docs/faq.md) | "Is this a rootkit?" "Can I uninstall it?" "Does it slow my PC down?" |

## For server owners

| Doc | What it answers |
|-----|-----------------|
| [Install](docs/server-owners/install.md) | Drop-in resource setup in minutes. |
| [Configuration](docs/server-owners/configuration.md) | Every convar, the secure defaults, and fail-mode behaviour. |
| [Database](docs/server-owners/database.md) | What is stored tenant-side and why it is your data, under your control. |
| [Troubleshooting](docs/server-owners/troubleshooting.md) | Common boot and connect issues. |

## Downloads

Official ZAC client releases are published **only** under this repository's
[Releases](../../releases) page. Every release ships:

- a **code-signed** installer (verify the publisher in the Windows dialog),
- a **SHA-256 checksum** so you can verify the download byte-for-byte,
- a **VirusTotal scan link** for full transparency.

**Never download ZAC from anywhere else.** The only canonical URL is
`https://z-hub.app/download`. <!-- TODO: confirm final canonical download URL --> If a server tells you to get
"ZAC" from another link, that link is not us.

## Reporting a security issue

Found a vulnerability or a way to bypass ZAC? Please report it responsibly — see
[SECURITY.md](SECURITY.md). We welcome and credit good-faith research.

---

*ZAC — Zero-knowledge Anti-Cheat. Built so the people we protect never have to
take our word for it.*
