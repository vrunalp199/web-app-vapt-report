# Web Application VAPT Report — AI Business Platform (Internship Assessment)

A two-phase Vulnerability Assessment & Penetration Testing (VAPT) engagement performed on a live, production AI-services marketing/lead-gen web application, as part of an authorized security internship assignment. Client/target identifying details have been redacted for public portfolio use — the full, unredacted report was delivered privately to the client.

##  Assessment Overview

| | |
|---|---|
| **Assessment Type** | Authorized Security Assessment (Internship Assignment) |
| **Phases** | Phase 1 – Reconnaissance & Initial Security Review · Phase 2 – Client-Side Code & Authentication Review |
| **Testing Style** | Passive reconnaissance + manual testing only — **no destructive testing or exploitation performed** |
| **Analysts** | Vrunal Pramod Patil, Junaid Rafique |
| **Target Type** | React SPA — AI automation / chatbot / LLM services marketing site with internship application intake |

##  Scope

- Website reconnaissance & technology fingerprinting
- Directory enumeration (Gobuster)
- `robots.txt` and sitemap analysis
- HTTP security header review (Burp Suite)
- General web server scan (Nikto)
- Authentication behavior review
- Client-side JavaScript bundle static analysis (keyword-based)
- File upload handling review (public form)

##  Methodology & Tools

- Passive reconnaissance
- Manual testing
- Browser Developer Tools
- Burp Suite (Community Edition)
- Gobuster
- Nikto
- Wappalyzer
- Client-side JS bundle static keyword review

##  Technology Stack Identified

React · React Router · Tailwind CSS · Framer Motion · AOS · Axios · Cloudflare CDN · LiteSpeed · HTTP/3

##  Key Findings Summary

| Finding | Severity |
|---|---|
| Chained: Login user-enumeration + missing rate limiting → targeted credential brute-force | **High** (chained) |
| JWT auth token stored in browser `localStorage` | Medium |
| Missing HTTP Strict-Transport-Security (HSTS) | Medium |
| Missing rate limiting / lockout / CAPTCHA on login | Medium |
| Admin route gated only by client-side token check (unconfirmed server-side) | Medium |
| User enumeration via differing login error messages | Low |
| Dev/localhost endpoints left in production JS bundle | Low |
| Unvalidated file upload on public application form | Low |
| Missing Referrer-Policy / X-Content-Type-Options / Permissions-Policy | Low |
| Minimal Content-Security-Policy configuration | Low |
| OTP-based signup/reset flow flagged for further dynamic testing | Informational |

### Highlight: Chained Enumeration + Brute-Force Risk
The login endpoint returned distinguishable error messages for registered vs. non-existent email addresses ("Invalid credentials" vs. "User not found"), allowing account enumeration. Combined with the absence of rate limiting, lockout, or CAPTCHA (10–20+ consecutive failed attempts accepted without throttling), this let an attacker confirm a valid, high-value account and proceed straight to an unthrottled password-guessing attack — elevating two individually Low/Medium issues into a single **High** severity risk.

**Remediation:** return identical generic errors for both cases, enforce per-account/per-IP rate limiting with progressive backoff, add CAPTCHA after repeated failures, and rotate/enable MFA on any account confirmed valid during testing.

##  Positive Security Practices Observed

- HTTPS enforced end-to-end
- Cloudflare CDN/reverse proxy in front of origin
- Sensitive files (`.git`, `.svn`, `.htaccess`) correctly return 403
- `robots.txt` present, blocks major AI crawlers
- Modern, actively maintained frontend stack
- CSP header present (baseline, room to strengthen)

## 📄 Report

The full report (PDF/DOCX) is in [`/report`](./report), including annotated screenshots for the authentication findings.

##  Disclaimer

This was an authorized, non-destructive reconnaissance and initial security review conducted under a supervised internship engagement. Several items are explicitly flagged in the full report as "requires further testing" rather than confirmed exploitable vulnerabilities. Target-identifying details (domain, contact email, screenshots) in this public version have been redacted or genericized; the complete report was provided privately to the client, who has since remediated the findings and approved this redacted summary for public portfolio use.

##  Authors

- Vrunal Pramod Patil
- Junaid Rafique

##  License

Consider adding a license (e.g., MIT for the write-up/methodology) — see [choosealicense.com](https://choosealicense.com/).
