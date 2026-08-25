# Network Security & Penetration Testing: Information Gathering & Footprinting

![Kali Linux](https://img.shields.io/badge/Kali_Linux-557C94?style=for-the-badge&logo=kali-linux&logoColor=white)
![Security Recon](https://img.shields.io/badge/Task-Footprinting_%26_OSINT-red?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

## Project Overview
This repository documents the technical methodologies, command execution outputs, and security findings for **Week 02: Information Gathering, Footprinting, and Perimeter Assessment**. 

The goal of this phase is to execute systematic passive and active reconnaissance against designated targets to establish domain ownership, trace DNS resolution paths, fingerprint application frameworks and web daemons, assess HTTP security policies, and identify Layer-7 perimeter defenses (Web Application Firewalls).

---

## Technical Toolchain & Methodology

| Phase | Tool | Objective |
| :--- | :--- | :--- |
| **Passive Reconnaissance** | `whois` | Extract domain registration lifecycle, registrar information, and authoritative nameservers. |
| **Active DNS Resolution** | `nslookup` | Perform active DNS queries to map hostnames to public IP addresses. |
| **Web Fingerprinting** | `whatweb` | Detect web server software, content management systems (CMS), and client-side dependencies. |
| **Header & Policy Analysis** | `curl` | Inspect HTTP response headers, cookie flags (`secure`, `HttpOnly`), and cache controls. |
| **WAF Identification** | `wafw00f` | Probe perimeter behavior to identify active Web Application Firewalls. |
| **OSINT Attack Surface Mapping** | `theHarvester` | Aggregate subdomains, public email contacts, ASN data, and IP ranges from public data sources. |

---

## Detailed Technical Execution & Evidence

### 1. Domain Registration & WHOIS Enumeration
* **Target Domain:** `networkwalks.com`
* **Registrar:** GoDaddy.com, LLC (IANA ID: 146)
* **Creation Date:** 2019-11-06
* **Registry Expiry Date:** 2027-11-06
* **Domain Status:** `clientDeleteProhibited`, `clientRenewProhibited`, `clientTransferProhibited`, `clientUpdateProhibited`
* **Authoritative Name Servers:** `NS6135.HOSTGATOR.COM`, `NS6136.HOSTGATOR.COM`, `NS29.DOMAINCONTROL.COM`, `NS30.DOMAINCONTROL.COM`

```bash
whois networkwalks.com


2. Active DNS Resolution & IP Mapping
Resolved Host Address: 192.232.216.135
 Active DNS Resolver: 8.8.8.8#53 (Google Public DNS resolver fallback utilized when local virtual gateway sockets timed out)
 Bash
nslookup networkwalks.com
3. Web Stack & Application Fingerprinting
HTTP Server: Apache  Content Management System (CMS): WordPress 7.1
Active CMS Plugins: WordPress Download Manager 3.3.58
Frontend Libraries & Tooling: Bootstrap 7.1, jQuery 3.7.1, Google Tag Manager
Administrative Email Exposed: info@networkwalks.com[cite: 2, 3]

Bash
whatweb networkwalks.com

4. HTTP Security Headers Inspection
Protocol & Status: HTTP/2 (200 OK)[cite: 2, 3]
Cookie Security Flags: Set-Cookie: __wpdm_client=...; secure; HttpOnly properly enforced[cite: 2, 3].
Permissions Policy: Configured for private-state token validation against Cloudflare, Google reCAPTCHA, and hCaptcha[cite: 2, 3].
Server Caching Engine: x-nginx-cache: WordPress, x-endurance-cache-level: 0[cite: 2, 3].
Bash
curl -I [https://networkwalks.com](https://networkwalks.com)

5. Web Application Firewall (WAF) Fingerprinting
Firewall Detection Status: Active WAF detected on target[cite: 2, 3].
Identified WAF Solution: ModSecurity (SpiderLabs)[cite: 2, 3]
Evaluation: Detected via HTTP response headers and block triggers across 2 probe requests[cite: 2, 3].
Bash
wafw00f networkwalks.com

6. Broad Attack Surface OSINT (theHarvester)
Passive OSINT reconnaissance was executed against enterprise infrastructure (microsoft.com) across search engines, certificate transparency logs (CRTsh, Certspotter), and threat feeds (Hudson Rock):
Associated ASNs: AS13335, AS133618, AS206834, AS40034, AS8070, AS8075
Public Email Contacts Harvested: dotnet-docker-bot@microsoft.com, opencode@microsoft.com, secure@microsoft.com
Discovered Public IP Addresses: 123 hosts enumerated  Discovered Subdomains & Hostnames: 9,966 endpoints mapped[cite: 1]
Bash
theHarvester -d microsoft.com -l 50 -b all

Security Analysis & Key Findings
Information Disclosure via Headers: The server discloses underlying software and CMS versions (Apache, WordPress 7.1, WordPress Download Manager 3.3.58)[cite: 2, 3]. Hiding server version banners reduces exposure to automated version-specific exploit targeting.
Perimeter Defense Effectiveness: The presence of ModSecurity WAF adds Layer-7 protection against unauthorized injection attacks and payload anomalies[cite: 2, 3].
DNS Redundancy: Authoritative nameservers are distributed across HostGator and DomainControl infrastructure, providing reliable DNS failover[cite: 2, 3].
