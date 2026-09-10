# Crisis Management Chronology & Regulatory Notifications — HydroRégie

**Project:** NIS2 Compliance Roadmap for HydroRégie
**Block:** 4 — Crisis Management & Notification
**Date:** [fill in]
**Author(s):** [fill in]
**Reference:** exercises the Block 3 architecture (`block3-architecture/`)
against a realistic incident scenario
**Diagram:** `incident-chronology.svg`

---

## 1. Scenario

This chronology is a **tabletop exercise scenario**, used to validate that
the segmented architecture from Block 3 and the incident-handling roadmap
item from Block 2 actually work under pressure, and to produce ready-to-use
notification drafts for HydroRégie's crisis team.

> An employee in the billing department opens a malicious attachment in a
> phishing email. Ransomware executes on their workstation and begins
> encrypting files on a shared drive in the Corporate IT zone.

## 2. Incident Chronology

| Time (from opening the attachment) | Stage | What happens |
|---|---|---|
| **T0 (Day 1, 08:00)** | Compromise | Malicious attachment opened; ransomware payload executes on a billing workstation |
| **T0+2h (10:00)** | Detection | EDR alert fires on the workstation; IT helpdesk opens a ticket and begins triage |
| **T0+4h (12:00)** | Qualification | Mass file-encryption behavior confirmed on a shared file server; the CISO is notified; the affected workstation and file server are isolated from the network; **incident declared** |
| **T0+4h to T0+6h** | Initial containment check | FW1 logs (IT/OT boundary) are reviewed: no anomalous connection attempts from the compromised segment toward the Industrial DMZ are found; the Industrial DMZ and OT/SCADA zone continue normal operation throughout |
| **T0+6h (14:00)** | Severity classification | Incident classified as **significant** under Article 23(3): severe operational disruption to administrative systems (billing, invoicing) even though water production is unaffected, plus initial uncertainty about potential cross-boundary spread. "Becoming aware" for the regulatory clock is set at T0+4h (12:00), when the incident was confirmed and declared |
| **T0+24h (Day 2, 12:00)** | **Early warning due** | Early warning submitted to the CSIRT/competent authority (see notification 1) |
| **T0+72h (Day 4, 12:00)** | **Incident notification due** | Full notification submitted with updated severity/impact assessment and indicators of compromise (see notification 2) |
| **T0+72h to T0+30d** | Remediation | Affected systems rebuilt from clean backups; all Corporate IT credentials reset; EDR coverage extended to remaining endpoints; MFA rollout (already a High-priority Block 2 action) accelerated for all remote and privileged access |
| **T0+72h+30d (~Day 34)** | **Final report due** | Final report submitted within one month of the incident notification (see notification 3) |

**Key outcome:** the segmentation validated in Block 3 performed as
designed — the Industrial DMZ and OT/SCADA zone were never affected, and
drinking-water production continued without interruption throughout the
incident. This is the concrete proof point that justifies the Block 3
architecture investment.

## 3. Notification 1 — Early Warning (within 24 hours)

Per Article 23(4)(a), the early warning must indicate whether the incident
is suspected to be malicious and whether it could have a cross-border
impact — it is a triage report, not a full analysis.

```
To: [National CSIRT / competent authority — ANSSI, France]
From: HydroRégie — Compliance Officer / CISO
Date/time: Day 2, 12:00 (24h after becoming aware)
Subject: Early warning — significant cybersecurity incident — HydroRégie (essential entity)

1. Entity identification
   Name: HydroRégie
   Sector: Drinking water supply (NIS2 Annex I)
   Status: Essential entity
   Registration reference: [MonEspaceNIS2 ID]

2. Nature of the incident
   Suspected ransomware infection originating from a phishing email,
   affecting workstations and a shared file server in the Corporate IT
   (administrative) network segment.

3. Suspicion of malicious/unlawful act
   Yes — indicators are consistent with a known ransomware family;
   forensic confirmation is in progress.

4. Cross-border impact
   None identified at this stage. HydroRégie's service area is limited to
   its local jurisdiction and no cross-border dependency has been
   identified.

5. Immediate containment measures taken
   Affected endpoints and file server isolated from the network;
   Industrial DMZ / OT-SCADA boundary (FW1) reviewed, no intrusion
   detected; drinking-water production and distribution are operating
   normally.

6. Next steps
   A full incident notification with a detailed impact assessment and
   indicators of compromise will follow within 72 hours of becoming aware,
   per Article 23(4)(b).
```

## 4. Notification 2 — Incident Notification (within 72 hours)

Per Article 23(4)(b), this must update the early warning with an initial
severity/impact assessment and, where available, indicators of compromise
(IoCs).

```
To: [National CSIRT / competent authority — ANSSI, France]
From: HydroRégie — Compliance Officer / CISO
Date/time: Day 4, 12:00 (72h after becoming aware)
Subject: Incident notification — significant cybersecurity incident — HydroRégie

1. Reference
   Follows early warning submitted on Day 2, 12:00.

2. Updated technical assessment
   Ransomware family: [to be confirmed by forensic analysis]
   Initial vector: phishing email with malicious attachment
   Systems affected: 1 workstation, 1 shared file server (Corporate IT
   zone only)
   Systems confirmed unaffected: Industrial DMZ, OT/SCADA zone, all
   water-treatment and distribution control systems

3. Severity and impact
   Operational impact: disruption to billing/invoicing functions;
   customer-facing water supply service not interrupted at any point.
   No personal data breach identified at this stage (to be confirmed by
   the ongoing forensic review; will be assessed jointly against GDPR
   notification obligations if applicable).

4. Indicators of compromise (IoCs)
   [File hashes, malicious sender domain, C2 IP addresses — to be
   attached as a technical annex once validated]

5. Mitigation measures taken since the early warning
   Affected systems remain isolated; forensic imaging completed; password
   reset in progress for all Corporate IT accounts; EDR alerting extended
   to additional endpoints; MFA deployment prioritized and accelerated.

6. Outstanding risks
   Full scope of data affected on the compromised file server still being
   determined. No indication of lateral movement toward the Industrial
   DMZ or OT/SCADA zone as of this notification.

7. Next steps
   A final report will be submitted no later than one month after this
   notification, per Article 23(4)(d).
```

## 5. Notification 3 — Final Report (within one month of the notification)

Per Article 23(4)(d), the final report must include a detailed description
of the incident (severity and impact), the type of threat/root cause,
applied and ongoing mitigation measures, and, where relevant, cross-border
impact.

```
To: [National CSIRT / competent authority — ANSSI, France]
From: HydroRégie — Compliance Officer / CISO
Date/time: ~Day 34 (within 1 month of the Day 4 notification)
Subject: Final report — significant cybersecurity incident — HydroRégie

1. Detailed description of the incident
   Full timeline from initial compromise (Day 1, 08:00) through detection,
   containment, and remediation, as documented in the internal chronology
   (see block4-crisis-management/incident-chronology.svg).

2. Severity and impact (final assessment)
   Confirmed impact limited to the Corporate IT zone: 1 workstation, 1
   file server, [X] files encrypted, no confirmed exfiltration of personal
   data. No impact on water production or distribution at any point.
   Estimated financial/operational cost: [to be completed].

3. Type of threat / root cause
   Root cause: successful phishing attack exploiting insufficient email
   filtering and lack of security awareness training (Block 2, measure
   (g), was not yet implemented at the time of the incident).
   Contributing factor: absence, at the time, of MFA on the affected
   account (Block 2, measure (j), in progress).

4. Applied and ongoing mitigation measures
   - Affected systems rebuilt from clean, verified backups
   - All Corporate IT credentials reset; MFA rollout completed for
     remote and privileged access
   - Email filtering rules strengthened; phishing-simulation training
     scheduled for all staff, including OT operators and management
   - Segmentation architecture (Block 3) confirmed effective: no
     evidence of any attempt to cross FW1 into the Industrial DMZ

5. Cross-border impact
   None identified.

6. Lessons learned / roadmap adjustments
   - Incident handling process (Block 2, measure (b)) formally adopted
     based on this real-world validation
   - Security awareness training (measure (g)) and MFA rollout (measure
     (j)) moved up in priority given their direct role in this incident
   - Next tabletop exercise scheduled to test an OT-targeted scenario
     specifically
```

## 6. Sources

- Directive (EU) 2022/2555, Article 23, in particular paragraphs (3) and
  (4)(a)–(d).
- ANSSI — incident reporting guidance for NIS2 entities (cyber.gouv.fr).
- ENISA — incident reporting technical implementation guidance.

---
*This concludes the four-block HydroRégie NIS2 compliance project. See the
root `README.md` for the full deliverable index.*
