# Security Hardening

## Context

The site handles customer PII and payment flows (via Razorpay), so the
infrastructure needed to go beyond a default WordPress install. This
covers the checklist applied across file access, transport security,
monitoring, and recovery.

## What was done

### 1. File and directory access
- Directory listing disabled via `.htaccess`
- Direct access to `wp-config.php` blocked
- `xmlrpc.php` blocked — a common target for brute-force and DDoS
  amplification attacks on WordPress sites, and not needed for this site's
  workflow
- Verified blocked paths return `403` rather than exposing file contents

### 2. Transport security
- HTTP security headers configured via `.htaccess`, targeting an **A**
  grade on [securityheaders.com](https://securityheaders.com) — covers
  headers such as `X-Content-Type-Options`, `X-Frame-Options`, and
  `Strict-Transport-Security`
- SSL/TLS set to Cloudflare's "Full" mode, encrypting traffic both
  browser → Cloudflare and Cloudflare → origin server

### 3. Application-layer protection
- **Wordfence** firewall and Web Application Firewall (WAF) enabled
- Malware scanning scheduled
- Two-factor authentication enabled on admin accounts
- Cloudflare's edge WAF as a second layer in front of the origin server

### 4. Email authentication
- SPF, DKIM, and DMARC configured across both mail providers in use
  (Hostinger transactional mail + Brevo), reducing the chance of spoofed
  mail being sent "from" the domain and improving deliverability of
  legitimate order/contact emails

### 5. Backups and recovery
- Automated **weekly backups** via UpdraftPlus
- Verified backup schedule is active and confirmed restore points exist,
  rather than assuming the schedule was running correctly

### 6. Uptime monitoring
- UptimeRobot configured on three endpoints: primary domain, `www`
  subdomain, and the checkout page specifically — since checkout failing
  is a distinct failure mode from the homepage going down, and it's the
  one most directly tied to lost revenue
- Alert contact verified with a live test notification, not just assumed
  to be configured correctly

### 7. Server configuration
- PHP memory limit, upload size, and execution time tuned for
  WooCommerce's needs (product image uploads, checkout processing) without
  leaving defaults that are either too restrictive or unnecessarily
  permissive

## Why this order

Access control and transport security were addressed first, since those
are the most direct attack surface. Application-layer protection
(Wordfence, WAF) came next as a second line of defense. Backups and
monitoring were treated as equally critical, not optional — the
assumption is that some layer of defense will eventually be tested, and
recovery/visibility need to already be in place when that happens rather
than being set up reactively.

## Verification, not assumption

Each item above was checked, not just configured — headers were tested
against securityheaders.com, blocked paths were confirmed to 403, backup
existence was confirmed rather than trusting the schedule toggle, and the
uptime alert was verified with an actual test notification. Configuration
that hasn't been verified to work isn't meaningfully different from no
configuration at all.
