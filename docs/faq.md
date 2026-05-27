# FAQ

Plain answers to the questions players actually ask. For the full detail, follow
the links into [Privacy](privacy.md) and the [Security model](security-model.md).

### Is ZAC a rootkit / kernel driver?

**No.** ZAC runs in **user-mode**, like any normal program. It installs no
ring-0 / kernel driver. You can see it in Task Manager, stop it, and uninstall
it. We chose user-mode deliberately — lower risk to your system, nothing that
can interfere with booting.

### What data does ZAC collect about me?

ZAC's central service collects **nothing that identifies you** — no name, Steam
ID, IP, or machine profile. Your PC scan stays on your PC; only a *clean /
suspicious verdict* is sent, and it goes to the **game server you joined**, not
to us. Full breakdown in [Privacy](privacy.md).

### Does ZAC read my files, chat, browser, or Discord?

**No.** No files, no chat, no browser history, no email. Reading your Discord
token is explicitly **forbidden** — that's malware behaviour and ZAC does not do
it.

### Can I uninstall it? What happens if I do?

Yes, anytime — it's a normal Windows program. With ZAC removed (or stopped), you
simply can't join servers that *require* it; they'll show a message pointing you
to the official download. Servers that don't require it are unaffected.

### Will ZAC slow down my PC or game?

ZAC is built to be lightweight and is user-mode. It does periodic work, not a
constant heavy load. If you ever see ZAC measurably hurting performance, please
[tell us](../SECURITY.md) — that's a bug.

### Do I need a TPM? I have an older PC.

ZAC prefers a TPM (the security chip Windows 11 already requires) because it
keeps your key safe in hardware. Servers can choose to require it. On machines
without one, ZAC can fall back to a software-protected key — but some servers may
not accept that tier. See the [security model](security-model.md#the-cryptographic-foundation).

### Do I need an account or to log in?

**No.** Enrollment is silent and anonymous — no account, no email, no login.

### Where do I download ZAC? How do I know it's the real one?

Only from the official Releases page linked in the [README](../README.md#downloads).
Every release is **code-signed**, ships a **SHA-256 checksum**, and includes a
**VirusTotal** link. If a server points you to ZAC from any other URL, don't
trust that link.

### Why is ZAC closed-source if you care about transparency?

Because secrecy of the *code* isn't what keeps you safe — cryptography is. We
publish the entire trust model and keep only the cheat-detection internals
closed. The reasoning is laid out in
[Why closed source, but transparent](security-model.md#why-closed-source-but-transparent).

### Can ZAC stop every cheat?

No, and we won't claim it. Cheats running on separate hardware (DMA cards,
capture-card aimbots) can't be *eliminated* by any anti-cheat — they're
**detected and mitigated**, not solved. We name these limits openly in the
[security model](security-model.md#honest-residuals).

### I'm a server owner — where do I start?

The [Install guide](server-owners/install.md).
