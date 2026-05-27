# Troubleshooting (server owners)

| Symptom | Likely cause & fix |
|---|---|
| `[zac] config error: zac_tenant_key convar is empty` | The convar block is **below** `ensure kernel_ac` instead of above it, or has a typo. Move it above the `ensure` line and restart. |
| `[zac] config error: zac_api_url must start with http://` | The URL lost its protocol prefix. Wrap the full value in quotes, including `https://`. |
| `[zac] config error: zac_fail_mode must be "open" or "closed"` | Set `zac_fail_mode` to exactly `open` or `closed`. |
| Connects always deny with "backend unreachable" | `zac_api_url` is wrong, a firewall blocks **outbound** HTTPS from FXServer, or the backend is down. Temporarily set `zac_fail_mode "open"` to confirm the rest of the gate is healthy. |
| Connects always **allow** even with no client running | You're in `zac_fail_mode "open"` **and** the backend is unreachable, so the gate is failing open. Check the console for `backend_unreachable` log lines and fix connectivity. |
| Player kicked "client not running" but their client *is* running | The client hasn't finished registering yet — have the player check the ZAC client's own status, then retry. |
| Steam-only players rejected, Discord-linked players accepted (or vice-versa) | An identity-strategy mismatch — make sure the identifiers your players present match what their client registered. |

## When to flip the fail-mode

If you're debugging connectivity, set `zac_fail_mode "open"` so players aren't
locked out while you work, then restore your intended policy once healthy. For
high-trust servers that prefer to lock the door during any outage, `closed` is
the right long-term setting — just know it trades availability for strictness.

## Still stuck?

- Re-read the [Install](install.md) and [Configuration](configuration.md) pages.
- Check the FXServer console for the `[zac] kernel_ac started — ...` banner; its
  values tell you exactly what config the resource actually loaded.
- For a suspected bug or security issue, see [SECURITY.md](../../SECURITY.md).
