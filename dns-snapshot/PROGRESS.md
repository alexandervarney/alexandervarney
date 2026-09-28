# Namecheap → Cloudflare move: progress

## Done (Phase 1, 2026-09-28)
- Snapshots in this folder, verified against Namecheap Advanced DNS screenshots.
- All six domains on Namecheap BasicDNS; DNSSEC off on all (no DS records).
- No Namecheap URL-redirect records anywhere.
- madplan.xyz: Supabase uses default `*.supabase.co` address (no custom domain).
- Namecheap email forwarding (eforward MX + SPF) on: alexandervarney.com, getdroma.com,
  madplan.xyz, panka.studio. Alexander sets up Cloudflare Email Routing after each goes Active.
- cathrine.co: Google Workspace MX (no SPF/DKIM/DMARC present).
- usejiffy.com: Resend (DKIM, send.* MX/SPF, DMARC), no inbound MX.
- Sandbox note: all port-53 traffic is intercepted by a caching resolver (TCP/53 blocked);
  retry on SERVFAIL. Direct authoritative queries are not possible here.

## Next (Phase 2)
- Cloudflare account exists; no zones yet.
- API token stored as an environment credential (Zone:Zone:Edit, Zone:DNS:Edit,
  Account Settings:Read, all zones in account). The proxy injects
  `Authorization: Bearer …` on requests to api.cloudflare.com, so call the API
  without an auth header; there is no env var to read. Test: GET /client/v4/user/tokens/verify.
- Create six zones (Free), reconcile auto-scan against snapshots, all records DNS-only,
  verify via Cloudflare NS, record assigned nameservers.
- Do not touch feels.design.

## Done (Phase 2, 2026-09-28)
- Six zones created (Free), account f036279fc9576200de563a75f954463a. Status: pending.
- Nameservers (all six): hans.ns.cloudflare.com, lia.ns.cloudflare.com
- Auto-scan found nothing; all 44 records added by API from the verified snapshot,
  all DNS-only. Exports in cloudflare/*.zone match snapshots exactly.
- Verified via API export, not dig: sandbox answers every query from public DNS
  regardless of @server, so Cloudflare NS can't be queried directly here.
- Token expires 2026-10-31.

## Phase 3 status
- usejiffy.com: NS switched 2026-09-28, zone ACTIVE, all 6 records served from Cloudflare (SOA hans.ns.cloudflare.com).
- alexandervarney.com, getdroma.com: NS switched, ACTIVE. cathrine.co, madplan.xyz, panka.studio: switched, pending.
- Namecheap Redirect Email has NO rules on any domain: Email Routing not needed.
  eforward MX/SPF records are inert; optional cleanup later.
- usejiffy.com confirmed loading in browser by Alexander.
  Sandbox proxy blocks curl to the sites; Alexander checks them in a browser.

## Next (Phase 3)
- Alexander switches nameservers in Namecheap; poll zone status via API.
- Phase 4 transfers: start with the three .com (all Active) before 2026-11-01; cathrine.co early (expires 2026-11-19).
  Auth codes go to alexandervarney@gmail.com.
