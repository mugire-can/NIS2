# Segmented IT/OT Network Architecture — HydroRégie

**Project:** NIS2 Compliance Roadmap for HydroRégie
**Block:** 3 — Network Architecture
**Date:** [fill in]
**Author(s):** [fill in]
**Reference:** implements the Critical-priority action from
`block2-gap-analysis/gap-analysis-roadmap.md`
**Diagram:** `architecture-diagram.svg` (exported from the project's
architecture diagram)

---

## 1. Objective

Today HydroRégie's management information system (IT) and industrial
control system (OT) share a single flat network. This design replaces it
with a **three-zone segmented architecture**, so that a compromise
originating in the office network cannot reach the systems that actually
control water treatment and distribution, and so that the water service can
keep running even if the corporate IT network is taken offline.

## 2. Zone Design

| Zone | Contents | Criticality | Network role |
|---|---|---|---|
| **Corporate IT** | Workstations, business applications (billing, HR, email), VPN gateway for remote/home-office access | Standard | User-facing, internet-connected, highest exposure to phishing/ransomware |
| **Industrial DMZ** | Historian (read-only data mirror), patch/antivirus relay, jump host for supervised remote access | Elevated | No system in this zone can initiate a connection *into* the OT zone; it only relays approved traffic |
| **OT / SCADA** | SCADA/HMI supervision, PLC network (pumping stations, treatment plants), field devices/sensors | **Maximum** | No direct or indirect route to the internet; the single most important zone to protect, since it directly controls drinking-water production |

This is the standard **Purdue-model-inspired** approach used in industrial
cybersecurity (ANSSI and ENISA OT guidance both recommend it): critical
control systems are placed behind at least two independent security
boundaries, with a DMZ acting as a controlled airlock rather than allowing
IT and OT to talk to each other directly.

## 3. Technical Controls at Each Boundary

### FW1 — IT/OT boundary (between Corporate IT and Industrial DMZ)

- **Dedicated firewall appliance**, distinct from any general-purpose
  office firewall, with a default-deny policy.
- Only a short, explicit allow-list of flows is permitted (e.g. historian
  data pull on a specific port, patch-relay downloads), each justified and
  logged.
- **VLANs** separate Corporate IT from the DMZ at Layer 2, so that even a
  misconfiguration at the switch level does not silently bridge the two
  networks.
- Remote access from outside HydroRégie terminates at the **VPN gateway**
  inside Corporate IT — it never has a direct path into the DMZ or OT zone.

### FW2 — DMZ/OT boundary (between Industrial DMZ and OT/SCADA)

- A **second, independently managed firewall** — not the same device or
  ruleset as FW1 — so a single misconfiguration cannot collapse both
  boundaries at once.
- **No inbound initiation from the DMZ into OT.** Where OT data must reach
  the DMZ (e.g. the historian), the connection is initiated *from inside OT
  outward* using a data-diode-like, one-way-preferred pattern, or a tightly
  scoped read-only proxy if bidirectional flow is unavoidable.
- **Jump host with session recording**: any human access to OT systems
  (vendor maintenance, engineering changes) goes through a single hardened
  jump host in the DMZ, with all sessions logged and, where the risk
  warrants it, recorded.
- **VPN access to OT is deliberately not exposed** — third-party
  maintenance access is scheduled, supervised, and time-boxed through the
  jump host rather than given standing VPN credentials.

### Inside the OT zone

- **Flat-network legacy risk mitigated further** by internal segmentation
  between the SCADA/HMI layer and the PLC/field-device layer where
  equipment allows it (e.g. separate VLANs per pumping station), limiting
  lateral movement even if the OT perimeter itself were breached.
- No general-purpose internet access, email, or removable-media policy
  gaps are permitted inside this zone; patches and antivirus signatures
  arrive only via the DMZ relay, after being vetted.

## 4. Why This Satisfies Article 21 Measures

| Article 21(2) measure | How this architecture contributes |
|---|---|
| (b) Incident handling | A compromise in Corporate IT can be contained at FW1, drastically shrinking the incident's blast radius before it reaches the water-production systems |
| (c) Business continuity | OT can keep operating on local control even if Corporate IT (and therefore billing, email, the office network) is fully shut down for remediation |
| (i) Access control / asset management | Each zone has its own, enforceable access policy; the jump host gives a single, auditable choke point for all human access to OT |
| (j) MFA / secure communications | The VPN gateway and jump host are the two points where MFA is enforced, covering both external and internal privileged access routes |

## 5. Continuity of the Drinking-Water Service

The design deliberately avoids creating a **single point of failure** for
service continuity:

- OT control loops (pumping, treatment) are designed to keep running
  autonomously even if the DMZ or Corporate IT zone is disconnected or shut
  down for incident response — this is why OT has no dependency on
  Corporate IT for its core control functions.
- The historian and patch relay in the DMZ are convenience/monitoring
  functions, not control-path dependencies: their failure or isolation must
  never be able to stop water production.
- This separation is what allows HydroRégie's crisis-management team
  (Block 4) to isolate and rebuild the Corporate IT zone during an incident
  **without** interrupting the water supply to the 450,000 inhabitants
  served.

## 6. Migration Notes (high level)

1. Physically and logically inventory all current IT/OT assets (feeds the
   Block 2 action on asset management).
2. Stand up the Industrial DMZ and FW2 first, since it protects the most
   critical zone; only then implement FW1 policy tightening on the IT side.
3. Migrate remote-access flows onto the VPN gateway + jump-host pattern
   before decommissioning any legacy flat-network access paths, to avoid an
   operational gap during the transition.
4. Validate with a tabletop exercise (feeding directly into the Block 4
   crisis-management chronology) before declaring the segmentation complete.

## 7. Sources

- Directive (EU) 2022/2555, Article 21(2).
- ANSSI — guidance on industrial control system (ICS/SCADA) security and
  network segmentation (cyber.gouv.fr).
- ENISA — OT/ICS cybersecurity guidance.

---
*Next: Block 4 (`block4-crisis-management/`) builds the incident
chronology and regulatory notifications on top of this architecture.*
