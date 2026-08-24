This repository contains the technical documentation,
execution logs, and evidence for Week 02: Information Gathering,
Footprinting, and Perimeter Assessment. The objective is to perform comprehensive passive and active reconnaissance against designated target domains to identify public attack surfaces, evaluate DNS configurations, 
uncover backend technology stacks,
and detect active Web Application Firewalls (WAF).

Toolchain & Methodology

whois networkwalks.com

2. DNS Resolution & IP Mapping
Resolved Host Address: 192.232.216.135[cite: 2, 3]

DNS Resolver: 8.8.8.8#53 (Public DNS fallback utilized when local virtual gateway dropped external queries)[cite: 2, 3]

Bash
nslookup networkwalks.com
3. Web Stack & Application Fingerprinting
HTTP Server: Apache[cite: 2, 3]

Content Management System: WordPress 7.1[cite: 2, 3]

CMS Plugins: WordPress Download Manager 3.3.58[cite: 2, 3]

Frontend Frameworks: Bootstrap 7.1, jQuery 3.7.1, Google Tag Manager[cite: 2, 3]

Public Contact Identified: info@networkwalks.com[cite: 2, 3]

Bash
whatweb networkwalks.com
4. HTTP Security Header Inspection
Protocol & Status: HTTP/2 (200 OK)[cite: 2, 3]

Cookie Attributes: __wpdm_client configured with HttpOnly and secure flags[cite: 2, 3]

Caching Layer: x-nginx-cache: WordPress, x-endurance-cache-level: 0[cite: 2, 3]

Permissions Policy: Configured for private-state token validation against Cloudflare and Google reCAPTCHA endpoints[cite: 2, 3]

Bash
curl -I https://networkwalks.com
5. Web Application Firewall (WAF) Fingerprinting
Perimeter Defense: Active WAF detected[cite: 2, 3]

Identified WAF: ModSecurity (SpiderLabs)[cite: 2, 3]

Detection Method: Evaluated behavioral responses and header patterns across 2 probe requests[cite: 2, 3]

Bash
wafw00f networkwalks.com
6. Broad Attack Surface OSINT (theHarvester)
Passive OSINT gathering was executed against large-scale infrastructure (microsoft.com) across public search indices, certificate transparency logs (CRTsh, Certspotter), and threat intelligence feeds (Hudson Rock)[cite: 1]:

Autonomous System Numbers (ASNs): AS13335, AS133618, AS206834, AS40034, AS8070, AS8075[cite: 1]

Discovered Public Emails: dotnet-docker-bot@microsoft.com, opencode@microsoft.com, secure@microsoft.com[cite: 1]

Harvested Public IP Addresses: 123 hosts[cite: 1]

Total Discovered Hostnames & Subdomains: 9,966 endpoints mapped[cite: 1]

Bash
theHarvester -d microsoft.com -l 50 -b all
Security Analysis & Recommendations
Minimize Information Disclosure: HTTP response headers leak precise software versions (WordPress 7.1, Apache, and caching plugins)[cite: 2, 3]. Configuring the server to suppress banner headers prevents automated vulnerability targeting based on version discovery.

Perimeter Hardening: The presence of ModSecurity WAF adds Layer-7 protection against SQL injection and cross-site scripting attacks before traffic reaches backend handlers[cite: 2, 3].

DNS Redundancy: Delegating authoritative DNS across multiple zones (HostGator and DomainControl) ensures domain availability and fault tolerance[cite: 2, 3].
