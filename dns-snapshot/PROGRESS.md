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
