# Network Findings — `google.com` (172.217.18.238)

> **Source data:** `ping google.com`, `nslookup` (from earlier), and two `nmap` scans provided by the user.
> **Scan timestamps:** `2025-10-22 17:46–17:49 WAT` (from Nmap output).

---

## Overview

This document summarizes open ports, detected services, and potential vulnerabilities based on passive `ping` results and the provided `nmap` scans for `google.com` (IP `172.217.18.238`).
Note: the data below is based **only** on the supplied outputs — no active scanning beyond that was performed here. For deeper verification (TLS/cipher checks, web app testing, etc.) obtain authorization before performing further tests.

---

## Quick summary

* **Resolved IP:** `172.217.18.238` (rDNS: `par10s10-in-f238.1e100.net`)
* **ICMP:** 105 packets sent, 105 received → **0% packet loss**. RTT min/avg/max = **124.8 / 213.2 / 832.9 ms** (high latency, variable).
* **Nmap results:** TCP ports **80** and **443** are **open**; both shown as `tcpwrapped`. Other TCP ports are reported as **filtered** / no-response.

---

## Open ports & detected services

|      Port |          State         | Detected service (nmap) | Notes                                                                                                             |
| --------: | :--------------------: | :---------------------- | :---------------------------------------------------------------------------------------------------------------- |
|    80/tcp |          open          | `tcpwrapped`            | HTTP accepted, but probe was wrapped/blocked — likely edge/load-balancer in front of service. No banner revealed. |
|   443/tcp |          open          | `tcpwrapped`            | HTTPS accepted, but probe was wrapped/blocked — TLS likely handled by edge infra (CDN/WAF/load-balancer).         |
| other TCP | filtered / no-response | —                       | 998 ports filtered per scan — likely firewall/edge policies in place.                                             |

**What `tcpwrapped` means:** Nmap couldn't fingerprint the application because the connection was accepted but the probe did not return a recognizable banner. Typical when probing systems behind TCP wrappers, reverse proxies, load balancers, or active probe blocking.

---

## Potential vulnerabilities & observations

> These are *hypotheses* and recommended checks — not confirmed vulnerabilities. Confirmation requires additional authorized testing.

### Port 80 (HTTP)

* **Potential risks:** Cleartext traffic if HTTP is served without redirect to HTTPS (possible privacy/leak risk).
* **Likelihood (google.com):** Very low — large providers typically redirect HTTP → HTTPS and enforce HSTS.
* **Recommended checks:** `curl -I http://google.com` to confirm redirect behavior and headers.

### Port 443 (HTTPS/TLS)

* **Potential risks:** Weak TLS ciphers, outdated protocol versions, or certificate problems could be issues if present.
* **Likelihood (google.com):** Extremely low — major providers maintain strong TLS configurations.
* **Recommended checks:** `openssl s_client -connect 172.217.18.238:443 -servername google.com` and `nmap --script ssl-enum-ciphers -p 443 172.217.18.238`.

### Edge infrastructure / WAF / CDN

* **Observation:** Many ports filtered and `tcpwrapped` services suggest traffic is fronted by load balancers, CDN, or WAF.
* **Implication:** Fingerprinting and direct version/OS enumeration is likely blocked. Misconfigurations in edge infra can create risks but no evidence here.

### ICMP / network performance

* **Observation:** Average RTT ~213 ms with spikes up to ~833 ms; 0% packet loss.
* **Implication:** Performance/latency issues; not a security vulnerability in itself.

---

## Risk assessment

* **Overall risk (from provided data):** **Low**
  Reason: only HTTP/HTTPS visible, both protected/obscured by edge infrastructure. No version banners or vulnerable services revealed from these scans.

---

## Recommended next steps (authorization required where noted)

> **Ethical/legal note:** Only perform intrusive scans on systems you own or have explicit permission to test.

### Non-intrusive / passive checks

* Check HTTP → HTTPS redirects and headers:

```bash
curl -I http://google.com
curl -I https://google.com
```

* View TLS certificate:

```bash
openssl s_client -connect 172.217.18.238:443 -servername google.com </dev/null | openssl x509 -noout -dates -subject -issuer
```

### TLS / cipher analysis (non-invasive but performs TLS handshakes)

```bash
nmap --script ssl-enum-ciphers -p 443 172.217.18.238
```

### Routing / latency investigation

```bash
traceroute google.com
# or
mtr --report google.com
```

### Deeper service/version enumeration (ONLY if authorized)

```bash
nmap -sV --version-intensity 5 -p 80,443 172.217.18.238
```

### Web application checks (ONLY if authorized)

* Tools: `nikto`, Burp Suite, `gobuster` (use responsibly and with permission).

---

## Example short report text<img width="1366" height="654" alt="VirtualBox_Kali Linux_22_10_2025_18_01_21" src="https://github.com/user-attachments/assets/aee1e13a-60ea-4be1-b3ea-7ecfbab64943" />

<img width="1366" height="654" alt="VirtualBox_Kali Linux_22_10_2025_18_02_13" src="https://github.com/user-attachments/assets/bb59c3ee-fa4d-4a03-b913-edf8cd192aac" />

> `google.com` resolves to `172.217.18.238`. ICMP shows 0% packet loss with variable/high latency (avg RTT ≈ 213 ms). Nmap shows TCP ports **80** and **443** open; both returned `tcpwrapped`, indicating edge infrastructure (CDN/WAF/load-balancer) is handling probes. 998 TCP ports reported as filtered. No application banners or software versions were exposed in the provided scans; therefore no explicit vulnerabilities can be confirmed from these outputs. Recommend passive TLS and header checks and, if authorized, a controlled TLS and web application assessment.
![Uploading VirtualBox_Kali Linux_22_10_2025_18_02_13.png…]()
<img width="1366" height="654" alt="VirtualBox_Kali Linux_22_10_2025_18_01_57" src="https://github.com/user-attachments/assets/cb86e3f2-ab9c-4be8-838a-4c88b1011f70" />
<img width="1366" height="654" alt="VirtualBox_Kali Linux_22_10_2025_18_01_21" src="https://github.com/user-attachments/assets/1edddbfe-5d78-4115-b683-62f864775fe0" />

---

## Notes & disclaimers

* This README is based **only** on the output (ping and nmap snippets). It is **not** a comprehensive security assessment.
* Running active scans against systems you do not own without permission may be unlawful or a violation of acceptable-use policies. Always obtain explicit authorization prior to further testing.
