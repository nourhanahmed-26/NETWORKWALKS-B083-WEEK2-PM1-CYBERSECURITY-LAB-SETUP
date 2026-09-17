# NETWORKWALKS-B083-WEEK2-PM1-CYBERSECURITY-LAB-SETUP
# Network Recon & Discovery – Lab Writeup

`Track: Cybersecurity Fundamentals | Engagement: Authorized Recon Exercise`


## Ground Rules

This writeup only documents testing done against a domain I had explicit permission to test, plus my own home network. Nothing here is a how-to for touching systems you don't own or haven't been authorized to test — running these same commands against a target without consent is illegal in most places and can carry real criminal and civil consequences. That responsibility sits with whoever runs the commands, not with this document.

## Why This Exercise

Before any real penetration test gets near exploitation, there's a discovery phase: figure out what's publicly knowable about a target, then figure out what's actually alive on a network. This lab walks through both halves — external footprinting of a domain, and internal host discovery on a LAN — to build that muscle memory.

## Method: External Footprinting

Target: `[your-authorized-target.com]`

| Step | Command | What it's for |
|---|---|---|
| Domain registration lookup | `whois [target-domain.com]` | Registrant, dates, name servers |
| Tech stack fingerprint | `whatweb [target-domain.com]` | CMS, plugins, server banner |
| DNS → IP resolution | `nslookup [target-domain.com]` | Get the resolved IP |
| Header inspection | `curl -I https://[target-domain.com]` | Server headers, exposed endpoints |
| WAF check | `wafw00f https://[target-domain.com]` | Detect a Web Application Firewall |
| DNS record dump | `dnsrecon -d [target-domain.com]` | NS / MX / TXT / SPF / SRV records |

**What I found (Tools used):**

- WHOIS → `[registrar / creation date / name servers]`
- WhatWeb → `[CMS + version, plugins, banner]`
- Nslookup → `[resolved IP]`
- Curl headers → `[notable headers / exposed paths]`
- Wafw00f → `[WAF present? which product?]`
- DNSRecon → `[NS/MX/TXT/SPF/SRV summary]`

## Method: Internal Host Discovery

Subnet scanned: `[10.x.x.x/24]` (my own network)

Using a GUI network scanner (e.g. Zenmap), I swept the subnet to enumerate live hosts.

- Hosts found: `[list of IPs]`
- MAC addresses: `[count/list]`
- Topology exported as `[PDF/etc.]` for the record



## What It Means: Risk Read-Out

| Finding | Where it came from | Why it matters | Severity |
|---|---|---|---|
| CMS/plugin version exposed | WhatWeb | Attackers can match versions to known CVEs | Medium |
| IP address resolvable | Nslookup | Basic infra location info | Low |
| HTTP headers/paths exposed | Curl | Aids further fingerprinting | Low |
| WAF fingerprintable | Wafw00f | Reveals part of the security stack | Low |
| DNS records enumerable | DNSRecon | Builds out infra profile | Medium |
| Multiple LAN hosts discoverable | Scanner | Rogue/unknown devices possible | Medium |

**Important caveat:** none of the above were exploited or validated as actual vulnerabilities — this was observation only. A finding here means "worth a closer look," not "confirmed weakness."

## What I'd Recommend

1. Audit what tech/version info is publicly discoverable and trim what isn't necessary.
2. Patch CMS/plugins against current advisories on a regular cadence.
3. Review HTTP response headers for unnecessary disclosure.
4. Periodically re-check DNS records for anything that shouldn't still be public.
5. Confirm WAF configuration matches actual intent (tuned, not just present).
6. Investigate any host on the LAN scan you don't immediately recognize.
7. Keep network topology docs current.
8. Only ever run recon/scanning against systems you own or are cleared to test.

## Takeaways

Passive and lightly-active recon alone — no exploitation — already surfaces a decent amount about a target: what it's running, where it lives, how it's fronted, and what's reachable on a local network. The other half of doing this well is writing it up clearly: what you ran, what came back, why it's relevant, and what to actually do about it.

