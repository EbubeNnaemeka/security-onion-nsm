# Alert Triage Log

Record each alert you triage using the structure below. The two scenarios are the planned test cases; the "Observed" fields get filled in when you run them.

## Scenario 1: Internal Port Scan

**Trigger:** `nmap -sV -p- 10.10.10.10` from an attacker VM against DC01.

**What to expect:**
- Suricata: `ET SCAN` signatures (e.g. Nmap scripting engine variants) during the scan window.
- Zeek `conn.log`: many short-lived connections (`S0`/`REJ` states) from one source across sequential ports in a short window.

**Observed Suricata signatures:** _fill in_
**Observed Zeek evidence:** _fill in_

**Triage reasoning:** The source is the lab's own attacker VM, so close as authorized test traffic. In a real environment, a full-port scan from an internal host points to post-compromise reconnaissance and should be escalated.

---

## Scenario 2: C2 Beaconing (replayed pcap)

**Source:** a public malware-traffic-analysis pcap containing known beacon traffic, replayed onto the sensor interface.

**What to expect in Zeek `conn.log`:** repeated connections to the same external IP at near-identical intervals, with small, consistent payload sizes. Normal browsing is bursty and variable, which is what separates it from a beacon.

**Kibana dashboards to use:** Connections (sort by duration/count) and DNS.

**Pcap used (name/source):** _fill in_
**Observed interval and destination:** _fill in_

**Triage reasoning:** In a live environment this pattern would call for host isolation and escalation to incident response.
