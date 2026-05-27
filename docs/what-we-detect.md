# What ZAC detects

ZAC targets the categories of cheating below. We describe *what* category each
covers — deliberately **not** the methods, signatures, or thresholds behind
them. Publishing the *how* would only hand cheaters an evasion recipe; it would
help no honest player or owner (see the [Disclosure Policy](../DISCLOSURE-POLICY.md)).

## Layer 1 — on the player's machine

The ZAC client inspects the local machine and reports a verdict (never the raw
findings — see [Privacy](privacy.md)). The categories it targets include:

- **Code injection into the game** — unauthorised code loaded into or
  manipulating the game process.
- **External overlays** — software drawing cheat information (ESP/visuals) over
  the game.
- **Capture-card / external-PC aimbots** — cheats that read the game from a
  second machine and feed back automated input. *Detected and mitigated; see
  [honest limits](#honest-limits).*
- **Hardware input automation** — devices and tools that inject scripted mouse
  or keyboard input (macro hardware, simulated-input devices).
- **Anomalous input topology** — input arriving from more sources than a normal
  setup presents.
- **Manipulated / injected game resources** — tampered or unauthorised
  client-side resource files.
- **Virtual-machine evasion** — environments used to hide or sandbox cheats.
- **Client tampering** — attempts to patch, hook, or disable the ZAC client
  itself. A tampered client cannot produce a "clean" verdict.

## Layer 2 — on the server, without trusting the client

The server independently watches game state the client cannot lie about. The
categories include:

- impossible **movement** (teleport, speed),
- **health / armour** anomalies (god-mode-style states),
- illegitimate **weapon / damage** behaviour,
- **economy** exploits (money/item duplication),
- abnormal **event rates** and unauthorised server events,
- server-side signs of **resource injection**.

Because this layer runs on the server, it catches a cheat **even if the client
is faked or lying** — and it catches things the on-PC scan can't see.

## Honest limits

We will not pretend to do the impossible. Two residuals are named openly in our
[security model](security-model.md#honest-residuals):

1. A local attacker asking their own genuine hardware key to sign "clean" while
   cheating.
2. Cheats running entirely on **separate hardware** (DMA cards, capture-card +
   hardware-mouse aimbots).

ZAC **detects categories of these and mitigates them**, and Layer 2 catches the
behaviour they produce — but no software on the victim's PC can *eliminate* a
cheat that doesn't run on that PC. No anti-cheat on the market eliminates them
either. This is a continuous arms race, and ZAC invests in it on both layers.

## Reporting evasion

If you've found a way to cheat past ZAC, that's exactly the kind of finding our
[security policy](../SECURITY.md) is for. Responsible reports are credited.
