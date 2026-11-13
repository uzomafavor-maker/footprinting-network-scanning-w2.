# Footprinting & Network Scanning: Week 2 Pentesting Project

![Status](https://img.shields.io/badge/status-completed-success)
![Phase](https://img.shields.io/badge/phases-1%20%26%202-blue)
![Platform](https://img.shields.io/badge/Kali%20Linux-Windows-informational)
![Scope](https://img.shields.io/badge/authorized%20testing-only-red)

**Author:** Favour Uzoma | Cybersecurity Intern, Networkwalks (Batch B084C)
**Modules:** W2-PM1 (Kali Linux footprinting) and W2-PM5 (Zenmap network discovery)
**Assessment date:** 13 November 2026

---

## Overview

This project documents the reconnaissance and scanning phases of a penetration test, carried out as part of the Networkwalks Cybersecurity Internship.

It covers two separate scopes:

1. **Passive and active footprinting** of an authorized public web target (domain, DNS, web technologies, WAF).
2. **Network discovery** of my own local LAN using Nmap/Zenmap.

The goal was not to "find bugs". It was to practise collecting evidence correctly and to separate **what a tool reports** from **what is actually a security conclusion**.

> **Note:** Target-specific details (domain, IP addresses, MAC addresses, software versions, screenshots and authorization reference) are intentionally redacted. The full report is confidential under the internship authorization.

---

## Scope & Authorization

| Item | Detail |
|---|---|
| Authorization | Written authorization letter issued by Networkwalks |
| Validity window | 08-14 November 2026 |
| Permitted | Limited, non-destructive reconnaissance and scanning |
| Prohibited | Exploitation, disruptive testing, third-party data access |
| In scope | Authorized training domain and its DNS; tester-owned LAN |
| Phases covered | Phase 1 (Reconnaissance/Footprinting), Phase 2 (Scanning/Discovery) |
| Out of scope | Phases 3-5, including exploitation |

**No exploitation was performed or documented.**

---

## Tools Used

| Tool | Purpose |
|---|---|
| Kali Linux | Platform for footprinting |
| Windows + Zenmap (Nmap GUI) | Local subnet discovery |
| `whois` | Domain registration details (registrar, dates, name servers, privacy) |
| `whatweb` | Fingerprint web technologies (server, CMS, plugins, libraries) |
| `nslookup` | Resolve the domain to an IP through DNS |
| `curl -i` | Inspect HTTP redirect behaviour and response headers |
| `wafw00f` | Detect whether a Web Application Firewall is in front of the site |
| `dnsrecon` | Enumerate DNS records (A, MX, TXT/SPF, SRV, SOA) |
| `ipconfig` | Identify local IP, subnet, gateway |

---

## Methodology

### Phase 1: Reconnaissance & Footprinting

```bash
whois <target-domain>
whatweb <target-domain>
nslookup <target-domain>
curl -i http://<target-domain>
wafw00f <target-domain>
dnsrecon -d <target-domain>
```

| Step | What I looked at | What I observed (generalized) |
|---|---|---|
| WHOIS | Registrar, creation/expiry dates, name servers, privacy | Registered through a commercial registrar with privacy protection; owner not identifiable from public data |
| WhatWeb | Server, CMS, plugins, JS libraries | HTTP redirects to HTTPS; Apache with a WordPress stack and common front-end libraries reported |
| nslookup | Name-to-IP resolution | Non-authoritative answer returned; a later resolver timeout appeared |
| curl -i | Redirect and headers | `301` to HTTPS; headers disclose server and WordPress-related redirect plugin |
| Wafw00f | WAF presence | A ModSecurity WAF fingerprint was reported |
| DNSRecon | A, MX, TXT, SRV, SOA | Standard mail/web records, an SPF policy, and cPanel autodiscovery SRV records; SOA query timed out |

### Phase 2: Scanning & Discovery (own LAN only)

```bash
ipconfig                         # identify local IP, mask, gateway
nmap -sn 192.168.x.0/24          # ping scan: live host discovery
nmap -T4 -A -v 192.168.x.0/24    # intense scan (output incomplete)
```

- The ping scan covered **256 addresses in about 3.6 seconds** and reported **six live hosts**.
- MAC vendor lookups labelled some devices and left others as "unknown".
- The intense scan output was **incomplete**, so I drew **no** port, service, OS or vulnerability conclusions from it.
- The Zenmap topology view showed **more nodes than the ping scan** (aggregated scan states), so I did not treat it as a confirmed host inventory.

---

## Findings Summary

These are **observations and validation priorities, not confirmed vulnerabilities.** No CVSS scores were assigned.

| # | Observation | Level | Why it matters |
|---|---|---|---|
| 1 | Technology fingerprints visible | Low | Can guide further research; must be verified first |
| 2 | Public DNS and hosting profile | Info | Normal public information, but it helps attackers map infrastructure |
| 3 | HTTP redirect headers disclose platform | Low | Limited technology disclosure; HTTPS headers were not assessed |
| 4 | WAF fingerprint identified | Info | Reveals a security control; effectiveness untested |
| 5 | DNS checks incomplete | Info | SOA and DNSSEC status remain unverified |
| 6 | Six LAN hosts responded | Info | Inventory needs reconciling; no rogue device established |
| 7 | Topology differs from discovery | Info | Aggregated views can cause inventory errors |

---

## Key Lessons

1. **A fingerprint is not a vulnerability.** Version strings from tools can be wrong (I saw an implausible library version reported) and need independent verification.
2. **"Unknown" is not "unauthorized".** An unrecognized MAC vendor is an inventory task, not an incident.
3. **A timeout is not a conclusion.** A resolver timeout does not prove a DNS failure, and "no answer for DNSSEC" does not prove DNSSEC is absent.
4. **Do not mix scopes.** A public website and a private LAN are separate assessments with separate evidence.
5. **Keep full scan exports.** Screenshots of partial output cannot support final findings. Save complete exports with consistent timestamps.
6. **Authorization comes first.** Every command was run inside a written scope and time window.

---

## Recommendations

**Web and DNS**
- Review what technology information is publicly visible (CMS, plugins, headers).
- Keep CMS platforms and plugins updated and track current advisories.
- Review HTTP response headers for unnecessary disclosure, including on HTTPS.
- Review DNS records periodically; expose only what is required.
- Verify WAF deployment with the administrator, maintain rules and logs, and test effectiveness only under separate authorization.

**Network**
- Run regular internal discovery scans of your own network.
- Investigate any unexpected device before assuming anything.
- Maintain current network documentation and topology records.

**Process**
- Perform security testing only with written authorization.

---

## Limitations

- Tool-reported versions and WAF fingerprints were not independently verified.
- The intense Nmap scan output was incomplete.
- HTTPS header configuration was not assessed.
- SOA and DNSSEC status were not confirmed due to resolver timeouts.
- No vulnerability validation or exploitation was performed (out of scope).

---

## Skills Demonstrated

`Reconnaissance` `OSINT` `DNS enumeration` `Web technology fingerprinting` `WAF detection` `Nmap / Zenmap` `Host discovery` `Evidence documentation` `Risk rating` `Professional reporting` `Ethical & authorized testing`

---

## Disclaimer

This work was done under written authorization for educational purposes within the Networkwalks Cybersecurity Internship. Do not run these techniques against any system you do not own or lack explicit written permission to test.

---

## Author

**Favour Uzoma** | Cybersecurity Intern, Batch B084C
GitHub: [uzomafavor-maker](https://github.com/uzomafavor-maker)
LinkedIn: [favour-uzoma](https://www.linkedin.com/in/favour-uzoma-226b27381)
