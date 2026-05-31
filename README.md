# Mobile Portal 📱

The mobile hub for riktom.com — **[m.riktom.com](https://m.riktom.com)**

A fast, lightweight landing page that serves as the mobile entry point for the entire
riktom.com app suite. Mobile visitors to `riktom.com` are redirected here (server-side,
via nginx User-Agent detection), then tap straight into any of the 15 responsive apps.

## How it works

- **Hub-only redirect:** Only `riktom.com` redirects mobile traffic → `m.riktom.com`.
  Individual app subdomains are already mobile-responsive and are linked directly from
  the hub (no redirect loops).
- **Desktop escape hatch:** The "View full desktop site" link sets a `nomobile=1` cookie
  (via `?nomobile=1`) so those users are never redirected again.
- **No JavaScript, no dependencies** — single static `index.html`, loads instantly.

## Apps linked

LocalHelp · RiverWatch · Fire Watcher · Hunt & Fish Forecast · Hunting Season Tracker ·
Deer Collision Radar · Ramp Radar · Burn Permit Checker · Field Reports ·
Truck Finder · Scoreboard · Family Fun Finder · Night Sky Guide ·
Outdoors Dashboard · Storm Desk

## Deploy

```bash
rsync -a --exclude='.git' -e "ssh -i ~/.ssh/riktom_vps" \
  ./ root@72.62.83.12:/opt/mobile-portal/
```

Served by nginx from `/opt/mobile-portal/` at `m.riktom.com`. Mobile detection lives in
`/etc/nginx/conf.d/mobile-redirect.conf` on the VPS (three `map` blocks); the redirect
itself is in the `riktom.com` server block.
