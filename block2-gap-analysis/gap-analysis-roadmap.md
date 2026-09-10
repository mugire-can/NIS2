# Gap Analysis & Compliance Roadmap — HydroRégie

**Project:** NIS2 Compliance Roadmap for HydroRégie
**Block:** 2 — Gap Analysis & Roadmap
**Date:** [fill in]
**Author(s):** [fill in]
**Reference:** builds on `block1-qualification/qualification-note.md`

---

## 1. Method

Each of the ten Article 21(2) risk-management measures is assessed against
HydroRégie's current situation as described in the case (no IT/OT
segmentation, essential entity serving 450,000 inhabitants, no documented
crisis or notification process). Each measure is scored on a 3-level scale:

- 🔴 **Not covered** — no policy, process or control exists
- 🟠 **Partially covered** — something exists but is incomplete, informal, or
  not tested
- 🟢 **Covered** — a documented, implemented and periodically reviewed
  control exists

Gaps are then prioritized using **impact × likelihood**, taking into account
that HydroRégie operates a service with direct public-health consequences
(drinking water).

## 2. Gap Analysis Table (Article 21(2)(a)–(j))

| # | Measure (Art. 21(2)) | Current state at HydroRégie | Status | Priority |
|---|---|---|---|---|
| (a) | Risk analysis & information system security policy | No formal risk analysis has been performed; no written ISSP (information system security policy) | 🔴 | High |
| (b) | Incident handling | No documented detection/triage/escalation process; reliance on ad hoc reaction | 🔴 | **Critical** |
| (c) | Business continuity, backup, disaster recovery, crisis management | No tested continuity plan for water production/distribution in case of a cyber incident | 🔴 | **Critical** |
| (d) | Supply chain security | No security requirements in contracts with OT/SCADA vendors and integrators | 🔴 | Medium |
| (e) | Secure acquisition, development & maintenance; vulnerability handling | No vulnerability management process on OT equipment (patch management, EOL inventory) | 🔴 | High |
| (f) | Effectiveness assessment of security measures | No audits, no KPIs/KRIs, no internal review cycle | 🔴 | Medium |
| (g) | Basic cyber hygiene & training | No security awareness program for staff (including OT operators) | 🔴 | Medium |
| (h) | Cryptography & encryption policy | No encryption policy; unknown whether SCADA/remote-access traffic is encrypted | 🔴 | Medium |
| (i) | HR security, access control & asset management | No formal asset inventory of IT/OT systems; access rights not reviewed; **flat network means access control cannot be enforced at the network layer** | 🔴 | **Critical** |
| (j) | MFA / continuous authentication, secured communications | No MFA in place, including for remote access to OT systems | 🔴 | High |

**Overall picture:** HydroRégie currently covers **0 of 10** Article 21
measures in a documented and tested way. This is expected for an entity that
has not previously been subject to sector-specific cybersecurity regulation,
but it means the roadmap below must be sequenced carefully rather than
attempted all at once.

## 3. Root-Cause Finding

Nine of the ten gaps above are made structurally worse by a single root
cause identified in Block 1: **the absence of network segmentation between
IT and OT.** Without segmentation:

- incident containment (b) is nearly impossible — an incident anywhere is an
  incident everywhere;
- business continuity (c) cannot be guaranteed, since OT depends on the same
  network as the potentially-compromised office IT;
- access control and asset management (i) cannot be enforced at the network
  layer, only at the application/host layer, which is far weaker.

This is why the technical architecture work (Block 3) is placed immediately
after this roadmap rather than at the end of the project — it is the
precondition for several other measures to become effective at all.

## 4. Prioritized Roadmap

Priorities: **Critical** (address in 0–3 months) → **High** (3–6 months) →
**Medium** (6–12 months).

| Priority | Action | Related measure(s) | Owner | Target date |
|---|---|---|---|---|
| Critical | Design and deploy IT/OT network segmentation (see Block 3 architecture) | (b), (c), (i) | IT/OT Infrastructure Lead | Month 3 |
| Critical | Draft and test an incident response plan, including a specific OT/SCADA compromise scenario | (b) | CISO / Security Officer | Month 2 |
| Critical | Build and validate a business continuity plan for water production & distribution (manual fallback procedures, backups, RTO/RPO) | (c) | Operations Director | Month 3 |
| High | Perform a formal risk analysis (assets, threats, vulnerabilities) covering both IT and OT | (a) | CISO / Security Officer | Month 2 |
| High | Deploy MFA on all remote access and privileged accounts (IT and OT) | (j) | IT Infrastructure Lead | Month 4 |
| High | Establish an OT vulnerability & patch management process, including an asset/EOL inventory | (e) | OT/SCADA Lead | Month 5 |
| Medium | Introduce supply-chain security clauses in vendor/integrator contracts (SCADA, telemetry, cloud) | (d) | Procurement + CISO | Month 6 |
| Medium | Launch a security awareness and training program for all staff, including OT operators and management (per Art. 20) | (g) | HR + CISO | Month 6 |
| Medium | Publish a cryptography/encryption policy and audit current traffic encryption | (h) | CISO | Month 8 |
| Medium | Set up an internal audit cycle and KPIs/KRIs to assess measure effectiveness | (f) | CISO | Month 10 |
| Ongoing | Register and keep information up to date on the national NIS2 platform (MonEspaceNIS2) | Registration | Compliance Officer | Month 1, then continuous |
| Ongoing | Governance: management body approval and oversight of the whole program, plus board-level cybersecurity training (Art. 20) | Art. 20 | Executive Management | Month 1, then quarterly review |

## 5. Governance of the Roadmap

- A **steering committee**, including the management body (as required by
  Article 20), should review progress monthly for the first six months, then
  quarterly.
- Each action owner reports status against the target date; slippage on any
  **Critical** item is escalated to the management body immediately, given
  the personal accountability introduced by Article 20.
- This roadmap will be revisited after the Block 3 architecture is finalized,
  since segmentation design choices may adjust the timeline of dependent
  actions (MFA rollout, asset inventory).

## 6. Sources

- Directive (EU) 2022/2555, Article 21(2)(a)–(j) and Article 20.
- ANSSI — NIS2 guidance (cyber.gouv.fr).
- ENISA — NIS2 risk-management guidance.

---
*Next: Block 3 (`block3-architecture/`) translates the Critical-priority
segmentation action into a concrete, justified network design.*
