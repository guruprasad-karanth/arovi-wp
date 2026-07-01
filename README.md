# Arovi Life — WooCommerce Storefront

**Live site:** [arovi.life](https://arovi.life)

A production WooCommerce store for **Arovi Life**, an authorised reseller of
WellWith India Pvt. Ltd.'s Sea Buckthorn wellness range — supplements,
skincare, and haircare sourced from Ladakh at 3,500m altitude. This repo
contains the custom theme code, plugin configuration, and infrastructure
setup for the storefront.

![Arovi Life Homepage](docs/screenshots/Homepage.png)

---

## What I built

This was an end-to-end redesign and hardening of an existing WooCommerce
store — covering front-end, e-commerce flow, and production infrastructure.

**Storefront & UX**
- Custom-styled theme built on Blocksy, with a defined design system
  (typography: Cormorant Garamond / Inter, custom color tokens)
- Redesigned Homepage, Shop, Single Product, Cart, and Checkout flows
- Custom footer, login/signup modal, search, and contact form
- Blog / content section with SEO-oriented long-form articles
- Product organization by wellness concern (Immunity, Skin & Hair, Energy,
  Digestion, Women's/Men's Wellness)

**E-commerce**
- WooCommerce checkout customization
- Razorpay payment gateway integration (UPI, cards, netbanking, wallets)
- Transactional email setup (order confirmations, contact form) via SMTP

**Infrastructure & Security**
- Migrated DNS to **Cloudflare** (CDN, WAF, proxying) with zero email downtime
- Configured **SPF, DKIM, DMARC** across two mail providers (Hostinger +
  Brevo) for deliverability and anti-spoofing
- `.htaccess` hardening — directory listing disabled, `wp-config.php` and
  `xmlrpc.php` access blocked
- HTTP security headers (targeting an A grade on securityheaders.com)
- **Wordfence** firewall, WAF, malware scanning, and 2FA
- Automated weekly backups via **UpdraftPlus**
- Uptime monitoring on homepage + checkout via **UptimeRobot**
- **GitHub Actions** CI/CD pipeline — auto-deploys `wp-content` to the
  production server via FTP on push to `main`

---

## Tech stack

![WordPress](https://img.shields.io/badge/WordPress-21759B?style=flat&logo=wordpress&logoColor=white)
![WooCommerce](https://img.shields.io/badge/WooCommerce-96588A?style=flat&logo=woocommerce&logoColor=white)
![PHP](https://img.shields.io/badge/PHP-777BB4?style=flat&logo=php&logoColor=white)
![Cloudflare](https://img.shields.io/badge/Cloudflare-F38020?style=flat&logo=cloudflare&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-2088FF?style=flat&logo=githubactions&logoColor=white)
![Razorpay](https://img.shields.io/badge/Razorpay-0C2451?style=flat&logo=razorpay&logoColor=white)

---

## Repo structure
> Note: WordPress core files are intentionally excluded — this repo tracks
> the custom code (`wp-content`) rather than the full CMS install.

---

## Screenshots

| Homepage | Shop | Checkout |
|---|---|---|
| ![Homepage](docs/screenshots/Homepage.png) | ![Shop](docs/screenshots/shop-page.png) | ![Checkout](docs/screenshots/checkout-page.png) |

**Mobile view:**

![Mobile view](docs/screenshots/mobile-view-site.png)

---

## About

Arovi is an authorised reseller of [WellWith India Pvt. Ltd.](https://arovi.life)
products. This project was built and maintained end-to-end — design,
development, e-commerce configuration, DNS/email infrastructure, and
security hardening.
