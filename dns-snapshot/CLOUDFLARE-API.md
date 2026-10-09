# Using the Cloudflare API from a Claude Code session

Paste this into a Claude Code cloud session running in the **Default** environment.

---

I manage DNS for my domains on Cloudflare and want you to make changes through the Cloudflare API.

**Auth:** my API token is stored as an environment credential. The proxy adds
`Authorization: Bearer …` automatically to requests to `api.cloudflare.com`, so call the
API with no auth header (there is no env var to read). Test it with:
`curl -sS https://api.cloudflare.com/client/v4/user/tokens/verify`

**Token permissions:** Zone:Zone:Edit, Zone:DNS:Edit, Account Settings:Read, all zones in
the account. It can't touch the registrar, Email Routing or Rules. It expires 2026-10-31.

**Account ID:** f036279fc9576200de563a75f954463a
**Nameservers (all zones):** hans.ns.cloudflare.com, lia.ns.cloudflare.com

| Domain | Zone ID |
|---|---|
| alexandervarney.com | 4cf936612b859a3378f03185c05d25b0 |
| cathrine.co | 69069f4a2b4f9c46968d9ccdd185352b |
| getdroma.com | 434c49f1645be6cb9c3807671ad3f456 |
| madplan.xyz | 250d2c3e885d206a472f6293e8e18764 |
| panka.studio | d0c7c4cf45de2c476f794575c04016a6 |
| usejiffy.com | b5bdb60b55daf0ac2fd32db8807debed |

**Rules:**
- Keep records **DNS-only** (`"proxied": false`) unless I ask otherwise; sites are on Vercel.
- cathrine.co uses Google Workspace mail (5 Gmail MX records): don't change MX there
  without asking. usejiffy.com sends via Resend (`send.` MX/SPF, `resend._domainkey`, `_dmarc`).
- Before changing or deleting a record, show me the current value and what you'll change.
- Verify changes by reading the records back from the API (`GET /zones/<id>/dns_records`
  or `/dns_records/export`). The sandbox intercepts DNS, so `dig @hans.ns.cloudflare.com`
  returns cached public answers, not Cloudflare's directly; TCP/53 is blocked; retry SERVFAIL.
- Don't touch feels.design (not on Cloudflare, left to expire).

Full history and snapshots: branch `claude/gallant-hypatia-nzlphd` in
`alexandervarney/alexandervarney`, folder `dns-snapshot/` (see `PROGRESS.md`).
