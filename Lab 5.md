# Passive vs Active Reconnaissance — Comparison

| **Aspect**               | **Passive Recon**                    | **Active Recon**         |
|-------------------------:|--------------------------------------|--------------------------|
| **Tool Examples**        | `whois`, `theHarvester`, search engines, certificate transparency logs | `nmap`, `ping`, `traceroute`, `curl` |
| **Risk Level**           | Low                                  | High                     |
| **Detection Possibility**| Minimal                              | High                     |

**Summary**

Use **passive reconnaissance** first to collect publicly available information quietly (WHOIS, OSINT, archived records, public certificates). It’s low-risk and unlikely to trigger alerts, making it ideal for initial profiling and hypothesis building. 
Use **active reconnaissance** when you need to confirm live details — open ports, service versions, TLS ciphers, or application behavior — but only with explicit authorization, because active probes (nmap, ping, web scanning) generate noise, are likely to be detected, and can disrupt services. 
In practice, combine both: passive methods to map and prioritize, active methods for controlled verification.
