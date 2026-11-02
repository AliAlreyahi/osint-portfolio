# DNS & Email Configuration Analysis — example.com

## 1. Project Overview

**Target:** example.com  
**Project Type:** Passive OSINT  
**Category:** DNS & Email Security Analysis  
**Purpose:** Educational research and practical OSINT training.

## 2. Methodology

The investigation used publicly available DNS information through `nslookup`.

The following DNS record types were examined:

- A
- AAAA
- NS
- MX
- TXT
- SOA

The research was limited to passive information gathering. No direct interaction with systems, intrusive testing, or unauthorized access was performed.

## 3. Findings

### Nameservers (NS)

Two Cloudflare nameservers were observed:

- elliott.ns.cloudflare.com
- hera.ns.cloudflare.com

**Observation:** The domain uses Cloudflare nameservers. This alone does not establish where the website is hosted.

### IPv4 Records (A)

Two IPv4 addresses were observed:

- 104.20.23.154
- 172.66.147.243

**Observation:** Multiple IP addresses may be associated with load balancing, CDN infrastructure, or other network configurations.

### Mail Exchange (MX)

The domain returned:

`0 .`

**Observation:** This is a Null MX record, indicating that the domain declares it does not accept email.

### SPF Record

The TXT records included:

`v=spf1 -all`

**Observation:** This SPF policy indicates that no sending hosts are authorized to send email using this domain.

### DMARC Record

The published DMARC policy included:

- Policy: reject
- Subdomain policy: reject
- DKIM alignment: strict
- SPF alignment: strict

**Observation:** The policy requests rejection of messages that fail DMARC authentication and alignment checks.

### SOA Record

The SOA record provided information about the DNS zone, including its primary nameserver and administrative parameters.

## 4. Security Observations

Based on the observed DNS records:

1. The domain publishes a Null MX record.
2. SPF explicitly disallows authorized sending hosts.
3. DMARC uses a reject policy.
4. Cloudflare nameservers are used.

These findings describe the published DNS configuration at the time of observation. They do not prove that every possible subdomain or external email service follows the same configuration.

## 5. Limitations

- DNS records can change over time.
- A DNS lookup does not reveal the complete infrastructure.
- The presence of a DNS record does not independently prove how a service is used.
- No active security testing was conducted.

## 6. Conclusion

This exercise demonstrates how publicly available DNS records can provide useful information about domain infrastructure and email security configuration.

It also highlights the importance of interpreting DNS findings carefully and avoiding conclusions that go beyond the available evidence.

## 7. Ethics

## 8. SOA Record Analysis

**Command:**

`nslookup -type=SOA example.com`

**Observed Results:**

| Field | Observed Value |
|---|---|
| Primary Name Server | elliott.ns.cloudflare.com |
| Responsible Mail Address | dns.cloudflare.com |
| Serial | 2418004112 |
| Refresh | 10000 seconds |
| Retry | 2400 seconds |
| Expire | 604800 seconds |
| Default TTL | 1800 seconds |

**Analysis:**

The SOA record identifies the primary nameserver and provides DNS zone management parameters, including the serial number and refresh, retry, and expiry intervals.

These values describe the published DNS zone configuration and do not reveal the complete internal infrastructure.

**Evidence Source:** Public DNS query using `nslookup`.

This research was conducted for educational purposes using publicly available DNS information.

No unauthorized access, intrusive scanning, or exploitation was performed.

**Disclaimer:** This is an independent educational exercise and is not affiliated with the owner of example.com.
