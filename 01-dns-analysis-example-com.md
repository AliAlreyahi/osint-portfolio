# DNS Reconnaissance Report — example.com

## 1. Project Overview

This project documents a passive DNS reconnaissance exercise on the publicly accessible domain `example.com`.

The objective was to identify publicly published DNS records and interpret what they reveal about the domain's DNS configuration and email-related policies.

**Target:** `example.com`  
**Assessment Type:** Passive OSINT  
**Date of Observation:** 2 November 2026  
**Tools Used:** Windows Command Prompt, `nslookup`

## 2. Scope and Methodology

The assessment was limited to publicly available DNS information.

The following DNS record types were queried:

- NS — Name servers
- A — IPv4 addresses
- AAAA — IPv6 addresses
- MX — Mail exchange configuration
- TXT — Text records, including SPF-related information
- DMARC — Email authentication policy
- SOA — Start of Authority information

All queries were performed using `nslookup`. No direct interaction with application services, authentication systems, or private infrastructure was conducted.

**Important observation:** The responses displayed `Non-authoritative answer`, indicating that the results were returned by a recursive DNS resolver rather than directly by an authoritative name server.

## 3. Findings

### 3.1 NS Records

**Command:**
```cmd
nslookup -type=NS example.com
```

**Observed values:**
- `hera.ns.cloudflare.com`
- `elliott.ns.cloudflare.com`

**Analysis:**

The domain publishes two Cloudflare name servers. This indicates that Cloudflare is involved in its authoritative DNS infrastructure.

These results alone do not establish who owns the domain or where its website is hosted.

### 3.2 A Records

**Command:**
```cmd
nslookup -type=A example.com
```

**Observed IPv4 addresses:**
- `172.66.147.243`
- `104.20.23.154`

**Analysis:**

The DNS response returned two IPv4 addresses for the domain.

Multiple returned addresses may be associated with CDN infrastructure, traffic distribution, or other DNS configurations. They do not necessarily represent two physical servers.

### 3.3 AAAA Records

**Command:**
```cmd
nslookup -type=AAAA example.com
```

**Observed IPv6 addresses:**
- `2606:4700:10::6814:179a`
- `2606:4700:10::ac42:93f3`

**Analysis:**

The domain publishes IPv6 addresses through AAAA records.

The returned address range is consistent with Cloudflare-associated infrastructure. This observation supports the presence of Cloudflare-related network services but does not independently establish the complete hosting architecture.

### 3.4 MX Records

**Command:**
```cmd
nslookup -type=MX example.com
```

**Observed value:**
- Preference: `0`
- Mail exchanger: `.` (root)

**Analysis:**

This is a Null MX configuration.

A Null MX record explicitly indicates that the domain does not accept email.

The preference value of zero does not indicate a high-priority mail server in this case, because the root value represents the absence of a mail exchanger.

### 3.5 TXT Records

**Command:**
```cmd
nslookup -type=TXT example.com
```

**Observed values:**
- An additional TXT value was returned.
- `v=spf1 -all`

**Analysis:**

The SPF record `v=spf1 -all` indicates that the published SPF policy authorizes no sending hosts for the domain.

The additional TXT value was not attributed to a specific provider or service because its purpose could not be established from the value alone.

### 3.6 DMARC Record

**Command:**
```cmd
nslookup -type=TXT _dmarc.example.com
```

**Observed value:**
```text
v=DMARC1;p=reject;sp=reject;adkim=s;aspf=s
```

**Analysis:**

The published DMARC policy contains the following settings:

- `v=DMARC1` — Identifies the DMARC protocol version.
- `p=reject` — Requests rejection of messages that fail DMARC evaluation.
- `sp=reject` — Applies a rejection policy to subdomains.
- `adkim=s` — Uses strict DKIM identifier alignment.
- `aspf=s` — Uses strict SPF identifier alignment.

Strict alignment requires an exact domain match for the relevant authentication identifier.

The published policy describes the requested handling of failing messages; it does not independently prove how every receiving mail server processes them.

### 3.7 SOA Record

**Command:**
```cmd
nslookup -type=SOA example.com
```

**Observed values:**

| Field | Value |
|---|---|
| Primary name server | `elliott.ns.cloudflare.com` |
| Responsible mailbox | `dns.cloudflare.com` |
| Serial number | `2418004112` |
| Refresh | `10000` seconds |
| Retry | `2400` seconds |
| Expire | `604800` seconds |
| Default TTL | `1800` seconds |

**Analysis:**

The SOA record provides administrative and timing information about the DNS zone.

The primary name server is listed as `elliott.ns.cloudflare.com`.

The responsible mailbox field uses DNS mailbox notation. It is not evidence of an individual's personal email address.

The timing values describe zone refresh, retry, expiry, and default TTL parameters.

## 4. Overall Assessment

The collected DNS records indicate that:

1. The domain publishes Cloudflare name servers.
2. Both IPv4 and IPv6 DNS records are present.
3. The domain publishes a Null MX record, indicating that it does not accept email.
4. Its SPF policy authorizes no sending hosts.
5. Its DMARC policy requests rejection of messages that fail DMARC evaluation, with strict SPF and DKIM alignment.
6. The SOA record provides DNS zone administration and timing information.

These observations describe the publicly published DNS configuration at the time of collection. They should not be treated as a complete assessment of the domain's infrastructure or security.

## 5. Limitations

- DNS records may change over time.
- Results were obtained through a recursive resolver and were marked as non-authoritative.
- No direct authoritative DNS queries were performed.
- No conclusions were made about physical server counts or internal infrastructure.
- No email delivery tests were conducted.
- No ownership verification or vulnerability testing was performed.

## 6. Ethical Statement

This exercise used publicly available DNS information only.

No unauthorized access, active vulnerability scanning, exploitation, authentication testing, or collection of private information was performed.

The project is intended for educational purposes and demonstrates a basic passive OSINT methodology.

## 7. Reproducibility

The following commands can be used to reproduce the DNS queries on Windows:

```cmd
nslookup -type=NS example.com
nslookup -type=A example.com
nslookup -type=AAAA example.com
nslookup -type=MX example.com
nslookup -type=TXT example.com
nslookup -type=TXT _dmarc.example.com
nslookup -type=SOA example.com
```

---

**Project Status:** Completed  
**Assessment Type:** Passive DNS Reconnaissance  
**Skill Demonstrated:** DNS Enumeration and Technical Report Writing
