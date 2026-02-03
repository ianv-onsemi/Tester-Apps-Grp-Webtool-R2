# Tester-Apps-Grp-Webtool-R2
Webtool for Tester Apps group. This group handles Test issues that are raised from Production meetings and escalated items from line maintenance. there can be issues that drops the Uptime [semi E10] of the ATE machines due to contacting, loadboards, testers, and other factors such as temperature or yield of device / product. 

Below is the **revised TAG Webtool outline** incorporating your new constraints and preferences. I preserved the parts you marked “all good” and tightened everything else to match: **SharePoint‑only storage, Teams notifications, XLS ingestion from Camstar, ITIL separation with TAG owning permanent fixes, DMAIC‑lite deliverables (1‑pager + supporting slides), and an Infobank of OPLs.**

***

## 1) Purpose & Scope (updated)

*   **Purpose:** Provide a **SharePoint‑centric** webtool that ingests **Camstar downtime logs (XLS)**, drives **fast triage**, and gives **TAG** a structured path from **Incident → Problem (RCA) → Change (permanent fix)** under ITIL discipline. Aligns metrics to **SEMI E10** (states, MTBF/MTTR, availability & utilization) and **SEMI E79/OEE** for test cells. [\[store-us.semi.org\]](https://store-us.semi.org/products/e07900-semi-e79-specification-for-definition-and-measurement-of-equipment-productivity), [\[technoprobe.com\]](https://www.technoprobe.com/wp-content/uploads/2022/01/OEE-for-Test-Floor-Managers-Engineers-and-Technicians.pdf), [\[manageengine.com\]](https://www.manageengine.com/products/service-desk/it-incident-management/incident-vs-problem-vs-change-vs-asset-management.html), [\[tagcrowd.com\]](https://tagcrowd.com/)
*   **Scope:** Automated test equipment (e.g., J750, FLEX, T2000, ETS364) in production; incidents from production meetings and LMT escalations; device yield/thermal issues that cause **Unscheduled Downtime (UDT)** or degrade performance/quality. E10’s six **mutually exclusive** states anchor all time accounting. [\[store-us.semi.org\]](https://store-us.semi.org/products/e07900-semi-e79-specification-for-definition-and-measurement-of-equipment-productivity), [\[support.google.com\]](https://support.google.com/tagmanager/answer/14918688?hl=en)

***

## 2) Roles & Responsibilities (revised)

*   **Production Operator / Line Maintenance (LMT)**
    *   Operate within their **existing environment**; continue logging on **Camstar**. Export **XLS** and upload to **SharePoint** “raw‑logs” library; optional comment tags. (No direct data entry in TAG Webtool.)
*   **Tester Applications Group (TAG)**
    *   **Owns escalations for permanent fix**. Performs triage on uploaded logs, promotes repeat/high‑impact items to **Problem (RCA)**, and drives **Change** through a **DMAIC‑lite** pack (1‑pager + supporting slides). Keeps E10/OEE metrics honest and comparable. Separation of Incident/Problem/Change follows ITIL best practice. [\[tagcrowd.com\]](https://tagcrowd.com/), [\[peergroup.com\]](https://www.peergroup.com/definition-of-standard/semi-e10/)
*   **PE/Yield / Product Owner / PMCAL**
    *   Collaborate on RCA data and approve recipe/limit/maintenance changes; consume dashboards and 1‑pagers.

***

## 3) Information Flow & Data Sources (new emphasis)

1.  **Camstar → XLS (shift/daily)** by Ops/LMT.
2.  **Upload XLS to SharePoint** (e.g., `Sites/TAGWebtool/RawLogs/`). File naming: `YYYYMMDD_Area_Cell_Tool_Shift.xlsx`.
3.  **TAG Webtool** (SharePoint-hosted app) parses XLS; maps events to **SEMI E10 states** and normalizes fields (tool, device, lot, cause, timestamps). [\[store-us.semi.org\]](https://store-us.semi.org/products/e07900-semi-e79-specification-for-definition-and-measurement-of-equipment-productivity)
4.  Parsed records are written to **SharePoint Lists** (not an external DB). Attachments remain in **SharePoint Libraries**.
5.  Dashboards & exports (Power BI or list views) reference the normalized SharePoint lists.

***

## 4) Modules (revised to your constraints)

### 4.1 Single Intake & Triage  *(reflects \[a1], \[b1], \[d1])*

*   **Ingestion**: Select an uploaded **XLS** from the **RawLogs** library; parser profile per tester family maps columns → E10 state changes, durations, and incident keys. [\[store-us.semi.org\]](https://store-us.semi.org/products/e07900-semi-e79-specification-for-definition-and-measurement-of-equipment-productivity)
*   **De‑duplication**: Merge overlapping rows; detect repeat symptoms (same tool/symptom window).
*   **Triage Queue**: Sort by **UDT minutes**, recurrence, product criticality; one‑click **Promote to Problem** (see 4.2).
*   **Evidence**: Link existing SharePoint files (photos/scope traces) from `Evidence/` library.

### 4.2 ITIL‑style Discipline (TAG owns permanent fix)  *(reflects \[a2])*

*   **Incident** (restore fast) records stay tied to logs; **Problem** (RCA) and **Change** (permanent fix) are distinct SharePoint list items with cross‑links to preserve ITIL separation. [\[tagcrowd.com\]](https://tagcrowd.com/), [\[peergroup.com\]](https://www.peergroup.com/definition-of-standard/semi-e10/)
*   **Promotion rules**: auto‑promote if (a) UDT > threshold or (b) ≥3 repeats in 7 days.

### 4.3 Change Control with DMAIC‑lite  *(reflects \[b2], \[d2], \[j1])*

*   **RCA 1‑Pager Generator** (from the Problem):
    *   Auto‑fill: Problem statement, **E10 impact** (UDT min, MTBF/MTTR trend), Top causes from Pareto, immediate containment. [\[store-us.semi.org\]](https://store-us.semi.org/products/e07900-semi-e79-specification-for-definition-and-measurement-of-equipment-productivity)
    *   Export as **PPT 1‑pager** (cover slide), plus **supporting slides** area for data (plots, screenshots).
*   **DMAIC‑lite Change Pack** (PPT):
    *   **Define** (1‑pager), **Measure** (E10 state/time series, OEE if applicable), **Analyze** (5‑Whys/Fishbone), **Improve** (solution options & risks), **Control** (run charts, audits). E79/OEE framing used when beneficial (A×P×Q). [\[technoprobe.com\]](https://www.technoprobe.com/wp-content/uploads/2022/01/OEE-for-Test-Floor-Managers-Engineers-and-Technicians.pdf), [\[manageengine.com\]](https://www.manageengine.com/products/service-desk/it-incident-management/incident-vs-problem-vs-change-vs-asset-management.html)
*   Approvals captured in SharePoint (people fields + timestamp); Teams post to TAG channel on status change.

### 4.4 Knowledge & Playbooks (“Infobank”)  *(reflects \[b3])*

*   **Infobank** SharePoint library of **1‑pager OPLs** (learning/procedure‑dependent fixes) and **Playbooks** by symptom.
*   Indexed by **Tool**, **Cause**, **Device/Flow**, **Environment (Thermal/Contact)**; surfaced as **contextual suggestions** in triage.
*   Each OPL links to related **Problems/Changes** and observed **MTBF improvement** post‑implementation (Control). (E10 metrics validate effectiveness.) [\[store-us.semi.org\]](https://store-us.semi.org/products/e07900-semi-e79-specification-for-definition-and-measurement-of-equipment-productivity)

### 4.5 E10/OEE Metrics Engine (SharePoint‑native)  *(no DB; reflects \[c1], \[h1])*

*   Compute **Availability, Utilization, MTBF, MTTR** from parsed E10 state durations; store daily rollups in a **SharePoint List**. [\[store-us.semi.org\]](https://store-us.semi.org/products/e07900-semi-e79-specification-for-definition-and-measurement-of-equipment-productivity)
*   Optional **OEE** views (A×P×Q) per device/cell; performance uses actual UPH vs target; quality from pass yield where available. [\[manageengine.com\]](https://www.manageengine.com/products/service-desk/it-incident-management/incident-vs-problem-vs-change-vs-asset-management.html), [\[technoprobe.com\]](https://www.technoprobe.com/wp-content/uploads/2022/01/OEE-for-Test-Floor-Managers-Engineers-and-Technicians.pdf)
*   Add **Functional Utilization** insight (distinguish Standby with WIP waiting) to link utilization with cycle time behavior. [\[intertekinform.com\]](https://www.intertekinform.com/en-ca/standards/semi-e79-2022-1036410_saig_semi_semi_3137047/)

***

## 5) Analytics & KPIs  *(you said: all good — preserved)*

*   **Core**: Availability/Utilization (E10), MTBF/MTTR, UDT Pareto, repeat‑issue heatmap. [\[store-us.semi.org\]](https://store-us.semi.org/products/e07900-semi-e79-specification-for-definition-and-measurement-of-equipment-productivity)
*   **OEE** (when applicable): Availability × Performance × Quality; show deltas pre/post change. [\[manageengine.com\]](https://www.manageengine.com/products/service-desk/it-incident-management/incident-vs-problem-vs-change-vs-asset-management.html), [\[technoprobe.com\]](https://www.technoprobe.com/wp-content/uploads/2022/01/OEE-for-Test-Floor-Managers-Engineers-and-Technicians.pdf)
*   **DMAIC‑lite**: Define→Control storyboard with KPI pins to each phase.

***

## 6) Platform, Storage & Notifications (revised hard constraints)

*   **Storage**: **Only SharePoint** (Lists for records, Libraries for files). No external DB or cloud storage. *(Reflects \[c1], \[h1])*
*   **Identity & Chat**: **Microsoft Teams** only for notifications (channel posts/mentions) and quick links. *(Reflects \[f1])*
*   **Upload pipeline**: Users drop **Camstar XLS** into `RawLogs/`; the app’s parser (SharePoint App) processes and writes to Lists. *(Reflects \[d1])*

> Notes on standards alignment for state handling and productivity/OEE are based on **SEMI E10** (state model, MTBF/MTTR, availability/utilization) and **SEMI E79** (productivity/OEE framing). [\[store-us.semi.org\]](https://store-us.semi.org/products/e07900-semi-e79-specification-for-definition-and-measurement-of-equipment-productivity), [\[technoprobe.com\]](https://www.technoprobe.com/wp-content/uploads/2022/01/OEE-for-Test-Floor-Managers-Engineers-and-Technicians.pdf)

***

## 7) UI & UX  *(you said: all good — preserved)*

*   **Cell Wallboard** (per area): live state tiles (E10 colors), shift uptime %, incident count; Top‑5 Losses.
*   **Incident/Problem/Change Cards**: timeline, evidence, RCA, DMAIC‑lite export.
*   **Infobank Drawer**: context‑sensitive OPLs/1‑pagers.

***

## 8) Governance & Data Quality  *(you said: all good — preserved)*

*   **E10 state discipline** (mutually exclusive), audit of open UDT spans; weekly data hygiene. [\[store-us.semi.org\]](https://store-us.semi.org/products/e07900-semi-e79-specification-for-definition-and-measurement-of-equipment-productivity)
*   **ITIL separation** of records; **Problem age** and **Change SLA** alerts via Teams. [\[tagcrowd.com\]](https://tagcrowd.com/)

***

## 9) Delivery Plan (4‑week pilot; updated Week 3)  *(reflects \[j1])*

**Week 1 – Foundations (SharePoint‑only)**

*   Provision SharePoint **Lists** (`Incidents`, `Problems`, `Changes`, `E10_Daily`, `Catalog_Causes`) and **Libraries** (`RawLogs`, `Evidence`, `Infobank`).
*   Build XLS parser (per tester family) and triage queue; Teams notifications wired.

**Week 2 – Metrics**

*   Compute **E10 rollups** (Availability, Utilization, MTBF/MTTR) from parsed logs; daily snapshots to `E10_Daily`. [\[store-us.semi.org\]](https://store-us.semi.org/products/e07900-semi-e79-specification-for-definition-and-measurement-of-equipment-productivity)
*   Draft Power BI (or SharePoint list views) for Shift/Day/Week trends.

**Week 3 – RCA & Change with DMAIC‑lite**  ✅ *(explicitly added)*

*   **Problem workspace** with 5‑Whys/Fishbone; **Quick RCA 1‑pager** generator.
*   **Change** entity gains **DMAIC‑lite pack export** (PPT: 1‑pager overview + supporting slides). OEE optional page when relevant. [\[technoprobe.com\]](https://www.technoprobe.com/wp-content/uploads/2022/01/OEE-for-Test-Floor-Managers-Engineers-and-Technicians.pdf), [\[manageengine.com\]](https://www.manageengine.com/products/service-desk/it-incident-management/incident-vs-problem-vs-change-vs-asset-management.html)

**Week 4 – Infobank & Training**

*   Infobank structure (OPL 1‑pagers) + contextual surfacing; finalize dashboards; operator/LMT refresher on **E10 states** and upload SOP.

***

## 10) SharePoint Structures & Templates (new)

*   **Libraries**
    *   `RawLogs/` – Camstar XLS uploads (write: Ops/LMT; read: TAG).
    *   `Evidence/` – scope shots, handler/contactor photos, logs.
    *   `Infobank/` – OPLs & Playbooks (PDF/PPT 1‑pagers).

*   **Lists**
    *   `Incidents` (ingested rows stitched to incidents; links to E10 intervals).
    *   `Problems` (RCA fields; links to Incidents, Evidence, Infobank items).
    *   `Changes` (DMAIC‑lite metadata; approvers; post‑change KPIs).
    *   `E10_Daily` (tool/date rollups: productive/standby/engineering/sched/unsched/nonsched, MTBF/MTTR, availability/utilization). [\[store-us.semi.org\]](https://store-us.semi.org/products/e07900-semi-e79-specification-for-definition-and-measurement-of-equipment-productivity)

*   **PPT 1‑Pager (auto‑generated)**
    *   **Title/Owner/Date**
    *   **Problem Statement & Scope**
    *   **E10 Impact**: UDT mins, Availability trend, MTBF/MTTR (sparkline) [\[store-us.semi.org\]](https://store-us.semi.org/products/e07900-semi-e79-specification-for-definition-and-measurement-of-equipment-productivity)
    *   **Immediate Actions** (containment)
    *   **Top Suspected Causes** (Pareto snapshot)
    *   **Next Step → DMAIC‑lite** (what/when/who)

*   **OPL (Infobank) 1‑Pager**
    *   Symptom → Cause → Standard Fix (images/steps), **Checks**, **Controls**, **References** (linked Problems/Changes).

***

## 11) KPI Definitions (unchanged, with standard citations)

*   **Availability/Utilization** by **SEMI E10** definitions; report Total/Operational as needed. [\[store-us.semi.org\]](https://store-us.semi.org/products/e07900-semi-e79-specification-for-definition-and-measurement-of-equipment-productivity)
*   **MTBF/MTTR** from UDT intervals and repair durations. [\[store-us.semi.org\]](https://store-us.semi.org/products/e07900-semi-e79-specification-for-definition-and-measurement-of-equipment-productivity)
*   **OEE** (E79 framing): **Availability × Performance × Quality**; used for comparative productivity views when applicable to test programs. [\[technoprobe.com\]](https://www.technoprobe.com/wp-content/uploads/2022/01/OEE-for-Test-Floor-Managers-Engineers-and-Technicians.pdf), [\[manageengine.com\]](https://www.manageengine.com/products/service-desk/it-incident-management/incident-vs-problem-vs-change-vs-asset-management.html)
*   **Functional Utilization** (standby with vs without WIP) available as an advanced diagnostic. [\[intertekinform.com\]](https://www.intertekinform.com/en-ca/standards/semi-e79-2022-1036410_saig_semi_semi_3137047/)

***
