# Install (server owners)

ZAC's server side is a single drop-in FiveM resource, `kernel_ac`. It gates
player connections and rechecks them while they play. There is **no
infrastructure for you to run** — you only hold a tenant key.

## Prerequisites

- A FiveM server (FXServer).
- A **tenant key** from ZAC onboarding (this identifies your server to ZAC).
- Outbound HTTPS from your FXServer host (so the resource can reach the ZAC
  backend).

## Steps

1. **Add the resource.** Copy the `kernel_ac` folder into your server's
   `resources/` directory so it sits at `resources/kernel_ac/`.

2. **Configure convars.** Open `server.cfg` and paste the block from
   [`config.example.cfg`](configuration.md#example-servercfg-block) **above** any
   `start` / `ensure` line that interacts with ZAC. Set at minimum:

   ```cfg
   set zac_tenant_key "your-tenant-key-here"
   set zac_api_url   "https://ac-api.z-hub.app"
   ```

3. **Start it.** Add `ensure kernel_ac` **after** the convars, then restart (or
   `refresh; ensure kernel_ac`).

4. **Confirm the boot banner.** You should see, in the FXServer console:

   ```
   [zac] kernel_ac started — backend=https://ac-api.z-hub.app  recheck=30000ms  fail_mode=open  http_timeout=5000ms
   ```

   If you instead see a `[zac] config error: ...` line, the resource refuses to
   start on purpose — fix the indicated cause and restart. Common causes are in
   [Troubleshooting](troubleshooting.md).

## Verify end-to-end

1. Connect a client **with the ZAC client running** → you load in normally.
2. Stop the ZAC client and reconnect → you're rejected with your
   `zac_kick_message_no_ac` message.
3. Connect with the client running, then kill it mid-session → you're dropped
   within roughly one `zac_recheck_interval_ms`.

If all three behave as above, ZAC is gating your server correctly.

## Next

- [Configuration](configuration.md) — every convar and the secure defaults.
- [Database](database.md) — what is stored, and that it's your data under your
  control.
- [Troubleshooting](troubleshooting.md).
