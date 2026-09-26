# Security Onion Network Security Monitoring Lab

A network-traffic-focused complement to the [SIEM detection lab](https://github.com/EbubeNnaemeka/siem-detection-lab): deploys Security Onion (Zeek + Suricata + Kibana) to monitor and triage network-level threats — port scans, C2 beaconing, and suspicious protocol usage — rather than host-level Windows telemetry.

## Why a separate stack from Splunk

The SIEM lab covers host/endpoint telemetry (Windows Event Logs, Sysmon). This lab covers the network layer — what a NOC/SOC analyst sees from wire traffic, independent of whether the endpoint is logging anything useful. Together they demonstrate coverage across both detection surfaces.

## Prerequisites

| Requirement | Details |
|---|---|
| Architecture | x86-64 only — does not run on ARM (Apple Silicon needs an x86 cloud VM or nested virtualization workaround) |
| Standalone mode | 4 CPU cores, **24GB RAM**, 200GB storage, 2 NICs |
| Eval mode (lighter) | 4 CPU cores, 8GB RAM, 200GB storage, 2 NICs — sufficient for this lab's traffic volume |
| ISO | [Security Onion 2.4](https://securityonion.net/) (free) |

Given the 24GB standalone requirement, this project ran in **Eval mode** to fit realistic home lab hardware — documented as a deliberate scope decision, not a limitation overlooked.

## Setup

1. Install Security Onion in Eval mode, single interface for management, second interface (promiscuous mode) as the monitoring sensor.
2. Span/mirror the AD lab's internal network traffic to the sensor interface (or route lab traffic through a bridge the sensor can see).
3. Generate traffic to analyze:
   - `nmap -sV -p- <target>` from an attacker VM against a lab target (port scan generation)
   - Replay a known-malicious pcap sample from a public malware-traffic-analysis repository for C2/beacon detection practice
4. Review alerts in the Security Onion **Alerts** dashboard (Kibana-based).

## Alerts to triage

Documented in [`alert-triage-log.md`](alert-triage-log.md):
- Suricata alert on an internal port scan (nmap `-sV` sweep) — full triage writeup
- Zeek `conn.log` analysis identifying long-duration low-volume connections consistent with C2 beaconing behavior

## Resume bullet (use once you have completed and verified the lab)

> Deployed Security Onion (Zeek/Suricata/Kibana) network security monitoring stack; triaged [N] alerts including port scans and C2-consistent beaconing traffic, documenting full analyst response for each.

## Repo contents

```
├── README.md
└── alert-triage-log.md
```
