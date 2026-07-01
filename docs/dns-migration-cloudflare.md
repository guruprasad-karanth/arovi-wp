# DNS Migration to Cloudflare

## Context

The site was originally hosted with DNS managed directly through Hostinger.
The goal was to move to Cloudflare for CDN, WAF, and DDoS protection —
without breaking email delivery or causing site downtime during the switch.

## Why this was non-trivial

The domain had several DNS records beyond the basic website A record:

- Mail routing (MX records) through Hostinger
- SPF, DKIM, and DMARC records for two separate mail systems — Hostinger
  mail and a third-party transactional email provider (Brevo, used for
  WooCommerce order emails and the contact form)
- Autoconfig/autodiscover CNAMEs for email client auto-setup

Any of these being dropped or mis-proxied during migration would have
silently broken order confirmation emails or triggered SPF failures,
causing legitimate emails to land in spam or bounce.

## Process

1. **Audited existing DNS** — recorded every A, AAAA, CNAME, MX, and TXT
   record from the Hostinger DNS panel before touching anything.
2. **Added the domain to Cloudflare** and reviewed its automatic DNS scan
   against the audit from step 1 to confirm nothing was missed.
3. **Found and manually added 3 records** the automatic scan missed —
   two DKIM CNAMEs and one verification TXT record for the transactional
   mail provider. Automatic scans don't always catch every record,
   especially ones tied to third-party services layered on top of the
   primary mail host.
4. **Set correct proxy status per record type**:
   - Proxied (orange cloud): root A/AAAA record, `www` CNAME — anything
     serving actual site traffic through Cloudflare's CDN
   - DNS only (grey cloud): MX records, all TXT records (SPF/DKIM/DMARC),
     and all mail-related CNAMEs — these must resolve directly, since
     proxying breaks mail server verification and autoconfig discovery
5. **Switched nameservers** at the registrar to Cloudflare's assigned pair.
6. **Verified post-migration**: confirmed the live site loaded correctly,
   checkout flow worked, and sent a test email through the contact form to
   confirm mail delivery was unaffected.

## Result

- Zero downtime during the switch
- Zero email disruption — confirmed via live test send/receive
- SSL/TLS mode set to "Full" (matches the existing origin certificate)
- Site now sits behind Cloudflare's CDN and WAF

## Key takeaway

DNS migrations that only check the website's A record are incomplete.
Mail authentication (SPF/DKIM/DMARC) is easy to overlook and is exactly
the kind of thing that fails silently — the site looks fine, but customers
stop getting order confirmations. Auditing every record type before and
after the switch, and treating mail records as DNS-only rather than
proxied, is what prevented this here.
