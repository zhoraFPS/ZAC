# Configuration (server owners)

Every `kernel_ac` convar, its default, and what it does. Convars must be set
**before** the `ensure kernel_ac` line — values set after the resource boots
take effect only on the next restart.

## Convar reference

| Convar | Required | Default | Purpose |
|---|---|---|---|
| `zac_tenant_key` | **Yes** | — | Your tenant API key from ZAC onboarding. Identifies your server. Sent as the `X-ZAC-Tenant-Key` header. |
| `zac_api_url` | **Yes** | — | ZAC backend base URL, no trailing slash. Production: `https://ac-api.z-hub.app`. |
| `zac_fail_mode` | No | `open` | What to do when the ZAC backend is unreachable. `open` = allow / keep players connected. `closed` = deny / drop. See below. |
| `zac_recheck_interval_ms` | No | `30000` | How often (ms) each connected player is re-checked. A player who kills their client mid-session is dropped within this window. |
| `zac_http_timeout_ms` | No | `5000` | Timeout (ms) for backend calls, enforced by the resource's own watchdog. |
| `zac_kick_message_no_ac` | No | `ZAC anti-cheat is not running.` | Shown when a player has no healthy ZAC client. Put your official download link here. |
| `zac_kick_message_banned` | No | `Banned. Ban ID: %s` | Shown when a player is banned. `%s` is replaced with the ban ID so support can reference it. |

## Choosing a fail-mode

This is the most important policy decision you make.

- **`open` (default)** — if ZAC's backend is unreachable, players are **allowed**
  through. This avoids a ZAC outage disconnecting your whole server. Best for
  most communities.
- **`closed`** — if the backend is unreachable, connections are **denied**.
  Choose this only for high-trust environments where you'd rather lock the door
  than risk a brief unguarded window.

`open` is the default deliberately: an outage that kicks every player off every
server at once is a worse outcome for most owners than a short gap in coverage.

## Secure defaults

ZAC ships safe out of the box. You generally only need `zac_tenant_key` and
`zac_api_url`; the rest have sensible defaults. As ZAC's TPM-backed attestation
rolls out, hardware-backed clients are required by default (the
[downgrade-resistant](../security-model.md#the-cryptographic-foundation) posture)
— this page will document the relevant convar when it ships.

## Example `server.cfg` block

```cfg
# ZAC — kernel_ac configuration. Paste ABOVE `ensure kernel_ac`.

# REQUIRED: your tenant API key from ZAC onboarding.
set zac_tenant_key "zac_REPLACE_ME"

# REQUIRED: ZAC backend base URL (no trailing slash).
set zac_api_url "https://ac-api.z-hub.app"

# OPTIONAL: re-check cadence for connected players (ms). Default 30000.
set zac_recheck_interval_ms 30000

# OPTIONAL: behaviour when the backend is unreachable: "open" or "closed".
set zac_fail_mode "open"

# OPTIONAL: backend call timeout (ms). Default 5000.
set zac_http_timeout_ms 5000

# OPTIONAL: shown when a player has no ZAC client running. Add YOUR download link.
set zac_kick_message_no_ac "ZAC anti-cheat is not running. Download it: https://z-hub.app/download"

# OPTIONAL: shown when a player is banned. %s = ban ID.
set zac_kick_message_banned "You are banned. Ban ID: %s. Appeal in our Discord."

# Start the resource (must come AFTER the convars above):
ensure kernel_ac
```
