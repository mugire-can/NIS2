# NIS2 Regulatory Qualification Note — HydroRégie

**Project:** NIS2 Compliance Roadmap for HydroRégie
**Block:** 1 — Regulatory Qualification
**Date:** [fill in]
**Author(s):** [fill in]

---

## 1. Purpose

This note establishes, on the basis of Directive (EU) 2022/2555 (NIS2), whether
HydroRégie falls within the scope of the directive, under which classification
(essential or important entity), and which core legal obligations follow from
that classification. It is the first deliverable of the compliance project and
the basis for the gap analysis produced in Block 2.

## 2. Entity Profile

| Attribute | Value |
|---|---|
| Name | HydroRégie |
| Activity | Public drinking water supply and distribution |
| Population served | 450,000 inhabitants |
| Legal status | Public water utility (régie) |
| IT/OT segregation | **None today** — IT (management information system) and OT (industrial control system: SCADA, pumping stations, treatment plants) share the same network, creating a single point of failure across the entire service |
| Confirmed status | Essential entity, per notification letter from the national competent authority |

## 3. Sector Classification under NIS2

NIS2 sorts regulated activities into two annexes:

- **Annex I — Sectors of high criticality**, which includes *Energy, Transport,
  Banking, Financial market infrastructure, Health, Drinking water, Waste
  water, Digital infrastructure, ICT service management (B2B), Public
  administration, and Space.*
- **Annex II — Other critical sectors**, which includes postal/courier
  services, waste management, chemicals, food, manufacturing, digital
  providers and research.

Drinking water supply and distribution is explicitly listed in **Annex I**.
HydroRégie's core activity therefore places it in a high-criticality sector
from the outset — the same tier as energy or health operators.

## 4. Size-Based Qualification (Article 2 / Article 3)

NIS2 uses a size-based cascade for Annex I sectors:

| Entity size (EU recommendation 2003/361) | Annex I sector | Classification |
|---|---|---|
| Large (≥250 employees **or** turnover > €50M **and** balance sheet > €43M) | Yes | **Essential entity** |
| Medium (50–249 employees **or** turnover/balance sheet €10M–€50M) | Yes | Important entity |
| Micro/small | Yes | Generally out of scope, unless a size-independent criterion applies |

Independently of size, Article 3 also designates as **essential** any entity
that is the **sole provider of a service critical for the maintenance of
vital societal functions or economic activities** in a Member State — a
criterion a public water utility serving 450,000 inhabitants is very likely
to meet, since there is typically no substitute distribution network for
drinking water in its service area.

**Conclusion:** Given its scale (a utility serving 450,000 inhabitants
necessarily operates with a workforce and budget well above the "medium
enterprise" threshold) and the essential, non-substitutable nature of
drinking water distribution, HydroRégie qualifies as an **essential entity**
on two independent grounds: (a) large-entity size within an Annex I sector,
and (b) sole-provider criterion for a vital societal function. This is
consistent with the qualification already confirmed by the national
authority.

## 5. Consequences of "Essential Entity" Status

Compared to "important" entities, essential entities are subject to:

- **Proactive supervision**: the national competent authority (in France,
  ANSSI) can audit HydroRégie at any time, without needing a prior indication
  of non-compliance (ex-ante supervision), whereas important entities are
  supervised ex-post (after an incident or a complaint).
- **Higher administrative fines**: up to €10,000,000 or 2% of total worldwide
  annual turnover, whichever is higher (vs. €7,000,000 / 1.4% for important
  entities).
- **Registration**: HydroRégie must register with the competent national
  platform (in France, *MonEspaceNIS2*) and keep its information (contact
  points, IP ranges, sector, etc.) up to date.
- **Same substantive obligations as important entities** on governance, risk
  management and incident notification (Articles 20, 21, 23) — the size of
  the obligation does not change with status, only the intensity of
  supervision and the level of sanctions.

## 6. Core Obligations Triggered (to be detailed in Block 2 and 3)

1. **Governance (Article 20)** — The management body must approve the
   cybersecurity risk-management measures, oversee their implementation, and
   can be held personally accountable for non-compliance. Members of the
   management body must follow cybersecurity training.
2. **Risk management measures (Article 21)** — Ten minimum measures covering
   risk analysis, incident handling, business continuity, supply-chain
   security, secure development/acquisition, effectiveness assessment, cyber
   hygiene and training, cryptography, access control/asset management, and
   MFA plus secure communications. This is the backbone of the Block 2 gap
   analysis.
3. **Incident notification (Article 23)** — Significant incidents must be
   reported to the CSIRT/competent authority following a strict timeline:
   early warning within 24h, incident notification within 72h, and a final
   report within one month. Detailed in Block 4.
4. **Registration** on the national NIS2 platform.

## 7. Specific Risk Highlighted by the Case

The absence of network segmentation between IT and OT is a direct and severe
non-conformity with Article 21(2)(a)/(d)/(i) (risk analysis, business
continuity, network security). It means that a compromise originating in the
office IT network (e.g. phishing, ransomware) could propagate to the
industrial control systems governing water treatment and distribution,
turning a routine IT incident into a **public health and safety incident**
affecting 450,000 people. This finding is the single highest-priority item
carried forward into the Block 2 gap analysis and the Block 3 architecture
work.

## 8. Sources

- Directive (EU) 2022/2555 of the European Parliament and of the Council of
  14 December 2022 (NIS2), in particular Articles 2, 3, 20, 21, 23 and
  Annexes I and II.
- ANSSI — official NIS2 information portal (cyber.gouv.fr).
- ENISA — NIS2 guidance and resources.

---
*This note qualifies HydroRégie's regulatory status only. Detailed gap
analysis against Article 21 measures is produced in Block 2
(`gap-analysis-roadmap.md`).*
