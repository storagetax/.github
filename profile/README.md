<div align="center">

<img src="https://raw.githubusercontent.com/storagetax/.github/master/profile/assets/hero.svg" alt="Storagetax — real-time storage right-sizing for cloud VMs" width="100%" />

### Stop paying for capacity you never use.

[**Book a 15-min demo →**](https://storagetax.lovable.app)

</div>

---

## The problem we solve

Cloud teams provision storage for the **worst hour of the year** and pay for it 8,760 hours a year.
A typical 1&nbsp;TB pool on Azure Premium SSD runs **$135/month**, regardless of whether you're using 80 GB or 800 GB of it.

Storagetax watches your VMs in real time, predicts headroom, and right-sizes storage **without downtime** — shrinking when you're idle, expanding the moment a surge starts, and reclaiming orphaned disks the moment a workload moves.

## What you get

<table>
<tr>
<td width="33%" valign="top">

### Live observability
Per-VM, per-mount, per-device metrics streamed over WebSocket. Spot a runaway log file before it pages your on-call.

</td>
<td width="33%" valign="top">

### Automatic right-sizing
Storage pools that grow during surges and shrink during quiet hours. No restart, no downtime, no scripts.

</td>
<td width="33%" valign="top">

### Bill back what you actually use
Per-team, per-tag dashboards showing capacity vs. used vs. paid. Drives the conversation that ends "we'll right-size next quarter".

</td>
</tr>
</table>

## How it works

1. **Install a 25&nbsp;MB agent** on each VM (one-click Azure VM Extension, AWS SSM, or `curl | bash`). The agent dials out only — no inbound ports, no SSH keys.
2. **Discover** your existing storage and mount points. You choose which to manage.
3. **Watch** live metrics flow in. Right-sizing kicks in within seconds of the first surge.

## Architecture (high level)

```
   Your VMs            Storagetax cloud
  +---------+           +-------------------+
  |  agent  |  ───────► |  orchestrator     |  ──► metrics TSDB
  +---------+   WSS     |  (ingest+route)   |
       ▲                +---------+---------+
       │                          │
       │                          ▼
       │                +-------------------+        +-------------+
       │                |    right-sizer    |  ◄───  |  dashboard  |
       │                |  (pool ops)       |        |  (you)      |
       │                +---------+---------+        +-------------+
       └────────────────── shrink / expand ──────────────────┘
                          (idempotent, no downtime)
```

Five focused services, one job each. None of them run in your environment except the agent.

## Trust by design

- **Agents dial out only.** Zero inbound ports. Zero SSH keys held by us.
- **Tenant isolation.** Every row is keyed by `client_id`; cross-tenant queries are impossible by construction.
- **Bring your own secrets.** Cloud credentials are stored encrypted, never logged, and used only to enumerate disks we're allowed to touch.
- **All operations are auditable.** Every expand, shrink, and reclaim is logged with the trigger reason and the agent that signed it.

## Tech we're built on

![Java 21](https://img.shields.io/badge/Java-21-007396?style=flat-square&logo=openjdk&logoColor=white)
![Spring Boot 3](https://img.shields.io/badge/Spring%20Boot-3.4-6DB33F?style=flat-square&logo=springboot&logoColor=white)
![MySQL 8](https://img.shields.io/badge/MySQL-8-4479A1?style=flat-square&logo=mysql&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-7-DC382D?style=flat-square&logo=redis&logoColor=white)
![InfluxDB](https://img.shields.io/badge/InfluxDB-2-22ADF6?style=flat-square&logo=influxdb&logoColor=white)
![Azure](https://img.shields.io/badge/Azure-supported-0078D4?style=flat-square&logo=microsoftazure&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-supported-FF9900?style=flat-square&logo=amazonaws&logoColor=white)
![GCP](https://img.shields.io/badge/GCP-supported-4285F4?style=flat-square&logo=googlecloud&logoColor=white)

## Try it

The product lives at **[storagetax.lovable.app](https://storagetax.lovable.app)**.
The source repositories in this org are private. If you'd like a tour of the internals,
[book a call](https://storagetax.lovable.app) and we'll walk you through it.

---

<div align="center">

Built by storage engineers who got tired of paying for empty disks.

[storagetax.lovable.app](https://storagetax.lovable.app)

</div>
