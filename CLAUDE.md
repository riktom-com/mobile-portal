# mobile-portal

Part of the riktom.com app suite. See https://github.com/riktom-com/handbook for full venture docs.

**Live:** https://m.riktom.com · **VPS path:** `/opt/mobile-portal/`

## Purpose

Mobile entry point for the whole riktom.com suite. Mobile User-Agents hitting `riktom.com`
are 302-redirected here by nginx; this hub then links directly to the 13 (already responsive)
app subdomains.

## Architecture

- **Single static file:** `index.html` (no build, no JS, no deps).
- **Redirect logic (server-side, on VPS):**
  - `/etc/nginx/conf.d/mobile-redirect.conf` — three `map` blocks:
    - `$ua_is_mobile` — matches phone User-Agents
    - `$send_to_mobile` — true only when mobile + no `?nomobile=1` arg + no `nomobile` cookie
    - `$set_nomobile_cookie` — sets persistent cookie when `?nomobile=1`
  - The redirect (`if ($send_to_mobile) { return 302 https://m.riktom.com; }`) lives in the
    `riktom.com` canonical server block, **not** here.
- **Hub-only:** App subdomains are NOT redirected (avoids loops; they're already responsive).

## Deploy

```bash
rsync -a --exclude='.git' -e "ssh -i ~/.ssh/riktom_vps" ./ root@72.62.83.12:/opt/mobile-portal/
```

## Editing the app list

Each app is a single `<a class="app">` block in `index.html`. To add/remove an app, copy an
existing block and update the icon, name, description, and `href` (points directly at the
app's subdomain).
