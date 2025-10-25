# 🧭 Cybersecurity Reconnaissance and Network Scanning — Summary Report

This document summarizes all practical tasks and outcomes from the session, including ethical vs. unethical hacking, open port detection, and reconnaissance comparison.

---

## 1. Ethical vs. Unethical Hacking

| **Case Study** | **Type** | **Justification** |
|----------------|-----------|-------------------|
| Case 1: A cybersecurity firm performs a penetration test on a company’s systems with written consent. | **Ethical (Authorized)** | The activity was approved and aims to identify and fix vulnerabilities. |
| Case 2: A hacker breaks into a bank’s database to steal customer data. | **Unethical (Unauthorized)** | Illegal intrusion with malicious intent and no authorization. |
| Case 3: A student scans their school’s network without permission to find open ports. | **Unethical (Unauthorized)** | Unauthorized access attempt, even if for curiosity. |

**Summary:**  
Ethical hacking is permission-based and used for defensive security. Unethical hacking is unauthorized and punishable under cybersecurity laws.

---

## 2. Network Scanning and Analysis (Ping & Nmap Results)

| **Scan Type** | **Target** | **Result Summary** |
|----------------|-------------|--------------------|
| `ping google.com` | `172.217.18.238` | Host reachable; average latency 213 ms; 0% packet loss. |
| `nmap google.com` | `172.217.18.238` | 2 open ports found: **80/tcp (HTTP)** and **443/tcp (HTTPS)**. Other 998 ports filtered. |
| `nmap -sS -sV 172.217.18.238` | `172.217.18.238` | Same ports open (80, 443); both reported as **tcpwrapped** — service protected behind firewall/load balancer. |

**Summary of Detected Services and Vulnerabilities:**

| **Port** | **State** | **Service** | **Potential Vulnerabilities / Notes** |
|-----------|------------|--------------|--------------------------------------|
| 80/tcp | Open | HTTP | May expose web server to vulnerabilities (e.g., misconfigurations, outdated CMS). |
| 443/tcp | Open | HTTPS | Encrypted traffic; risk depends on SSL/TLS configurations. |
| — | — | — | Other ports filtered; possible IDS/IPS protection in place. |

**Overall Assessment:**  
Google’s network is secured with minimal exposed ports and likely strong firewall rules. The services detected are standard and not inherently vulnerable without further analysis.

---

## 3. Passive vs. Active Reconnaissance Comparison

| **Aspect** | **Passive Reconnaissance** | **Active Reconnaissance** |
|-------------|-----------------------------|----------------------------|
| **Definition** | Collects information without direct interaction with target systems. | Gathers data by directly interacting with target systems. |
| **Tool Examples** | `whois`, `theHarvester`, `Shodan`, public databases. | `nmap`, `ping`, `traceroute`, `curl`. |
| **Risk Level** | Low | High |
| **Detection Possibility** | Minimal | High |
| **Interaction Level** | Indirect, no contact with target. | Direct communication with target systems. |
| **Use Case** | Early stage of reconnaissance to build a profile silently. | Later stage to verify information or map vulnerabilities. |

**Summary:**  
Passive reconnaissance is stealthy and ideal for the first phase of an assessment. Active reconnaissance, while riskier, is crucial for verifying and mapping live targets accurately.

---

## 4. Overall Cybersecurity Summary

| **Phase** | **Description** | **Tools Used** | **Outcome** |
|------------|----------------|----------------|--------------|
| Information Gathering | Identify targets and collect preliminary data. | `whois`, `theHarvester` | Basic domain and contact info collected. |
| Network Scanning | Detect live hosts and open ports. | `nmap`, `ping` | Found ports 80 and 443 open on Google server. |
| Service Enumeration | Identify service versions and configurations. | `nmap -sV` | Detected tcpwrapped services; limited details due to security filters. |
| Analysis & Reporting | Evaluate risks and summarize findings. | Manual interpretation | No vulnerabilities found; secure environment confirmed. |

---

### ✅ **Final Conclusion**

- **Ethical hacking** ensures systems are tested safely and lawfully.  
- **Reconnaissance** is a vital first step — use **passive** methods for stealth and **active** methods for validation.  
- **Google.com’s network** appears secure, with only essential ports (HTTP/HTTPS) exposed and likely protected by load balancers/firewalls.  
- Proper documentation of findings, like this report, helps improve transparency, compliance, and continuous learning.

---

**Author:** Ameen  
**Platform:** Kali Linux  
**Date:** October 2025  
**Tools Used:** `ping`, `nmap`, `whois`, `theHarvester`
