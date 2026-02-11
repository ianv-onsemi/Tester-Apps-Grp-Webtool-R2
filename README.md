
# Tester-Apps-Grp-Webtool-R2  
A webtool for the Tester Applications Group (TAG). The tool supports fast triage, structured RCA, and permanent-fix workflows for issues affecting Automated Test Equipment (ATE) uptime and productivity.

---

## 1) Purpose & Scope

### **Purpose**
Provide a **SharePoint‑centric webtool** that ingests **Camstar downtime logs (XLS)** and enables TAG to execute a structured flow from:

**Incident → Problem (RCA) → Change (Permanent Fix)**

Aligned to:
- **SEMI E10** — equipment states, MTBF/MTTR, availability, utilization  
- **SEMI E79/OEE** — productivity and effectiveness metrics  

### **Scope**
- Platforms supported: **J750, FLEX, T2000, ETS364**  
- Sources: Production meeting escalations, Line Maintenance escalations  
- Issues include: contacting, loadboard wear, tester faults, handler/prober issues, thermal drift, device-level yield excursions  
- E10’s six **mutually exclusive states** form the basis for all time accounting.

---

## 2) Roles & Responsibilities

### **Production Operator / Line Maintenance (LMT)**
- Continue using **Camstar** for downtime event logging  
- Export downtime **XLS** files and upload them to the SharePoint **RawLogs** library  
- No direct entry in the webtool

### **Tester Applications Group (TAG)**
- Owns escalations that require **permanent fixes**  
- Performs triage on uploaded logs  
- Promotes repeat/high‑impact incidents to **Problem (RCA)**  
- Drives **Change** through the **DMAIC‑lite** workflow (1‑pager + supporting data slides)  
- Maintains consistency of E10/OEE metrics and reporting

### **PE/Yield / Product Owner / PMCAL**
- Provide supporting analysis  
- Approve program/limit/maintenance changes  
- Consume dashboards and RCA documentation

---

## 3) Information Flow & Data Sources

1. **Camstar → XLS export** (shift or daily) by Ops/LMT  
2. **Upload XLS** to SharePoint:  
   `Sites/TAGWebtool/RawLogs/`  
3. TAG Webtool parses XLS, normalizes fields, and maps time to **E10 states**  
4. Parsed data stored in **SharePoint Lists** (no external DB)  
5. Dashboards (SharePoint views or Power BI) use the normalized lists

---

## 4) Modules

### 4.1 Single Intake & Triage
- Select XLS file from **RawLogs**  
- Parser profile per tester family maps columns → E10 state intervals  
- De‑duplicate overlapping logs  
- Triage queue ranks items by:
  - UDT minutes  
  - Recurrence  
  - Product criticality  
- Evidence may be linked from the **Evidence** SharePoint library

---

### 4.2 ITIL‑Style Discipline (TAG owns permanent fix)
- **Incident:** tied to the raw downtime data  
- **Problem (RCA):** created when UDT exceeds threshold or when recurrence criteria is met  
- **Change:** permanent fix, documented using DMAIC‑lite  
- All three exist as **separate SharePoint list items** with cross‑links  
- Preserves ITIL separation of responsibilities and workflow integrity

---

### 4.3 Change Control with DMAIC‑Lite

#### **RCA 1‑Pager Generator**
Auto‑fills:
- Problem statement  
- E10 impact (UDT, Availability, MTBF/MTTR trend)  
- Top causes (Pareto)  
- Immediate containment actions  

Output: **PPT 1‑pager** + space for supporting data slides

#### **DMAIC‑Lite Change Pack (PPT)**
- **Define** – 1‑pager summary  
- **Measure** – E10 state plots, trends, optional OEE  
- **Analyze** – Fishbone / 5‑Whys  
- **Improve** – options, risks, recommended solution  
- **Control** – run charts, audits, stabilization plan  

Approvals captured in SharePoint; Teams sends notifications.

---

### 4.4 Knowledge & Playbooks (Infobank)
- SharePoint library of **OPLs (1‑pagers)** and Playbooks  
- Indexed by Tool, Symptom, Cause, Device/Flow  
- OPLs surface contextually during triage  
- OPLs link back to relevant Problems/Changes  
- E10 improvements validate effectiveness

---

### 4.5 Metrics Engine (E10 / OEE)
- Compute Availability, Utilization, MTBF, MTTR from E10 state intervals  
- Daily rollups stored in SharePoint List `E10_Daily`  
- Optional **OEE** (A × P × Q) views using:
  - Performance: actual vs target UPH  
  - Quality: yield %  
- Supports functional utilization (standby with/without WIP)

---

## 5) Analytics & KPIs
- Availability, Utilization (E10)  
- MTBF/MTTR  
- UDT Pareto  
- Repeat‑issue heatmap  
- OEE (when applicable)  
- DMAIC‑lite KPIs pinned to each phase

---

## 6) Platform, Storage & Notifications

- **Storage:** Only SharePoint (Lists + Libraries)  
- **Identity & Chat:** Microsoft Teams (notifications & links)  
- **Upload pipeline:** XLS dropped into `RawLogs` → parsed → Lists updated  

---

## 7) UI & UX

- **Cell Wallboard:** E10‑colored state tiles, shift uptime %, incident count, Top‑5 losses  
- **Incident/Problem/Change cards:** timeline, evidence, RCA, DMAIC‑lite export  
- **Infobank drawer:** contextual OPL suggestions  

---

## 8) Governance & Data Quality

- E10 state discipline enforced (mutually exclusive)  
- Audit open UDT spans weekly  
- QA check for missing timestamps  
- ITIL separation monitored through Problem age & Change SLAs  
- Teams alerts for overdue items

---

## 9) Delivery Plan — 4‑Phase Pilot


### **Phase 1 – Foundations**

#### 1.1 SharePoint Lists & Libraries Setup
- [ ] Create SharePoint site `Sites/TAGWebtool` (if not exists)
- [ ] Create document libraries:
  - [ ] `RawLogs/` – configure versioning, 500MB max file size
  - [ ] `Evidence/` – enable thumbnail view for images
  - [ ] `Infobank/` – create folder structure: OPLs/, Playbooks/, Templates/
- [ ] Create SharePoint Lists with columns:
  - [ ] `Incidents` – ToolID, StartTime, EndTime, E10State, UDT_Minutes, RawLogID (lookup)
  - [ ] `Problems` – ProblemID, LinkedIncidentID, RCA_Status, Owner, CreatedDate
  - [ ] `Changes` – ChangeID, LinkedProblemID, DMAIC_Phase, Approval_Status
  - [ ] `E10_Daily` – Date, ToolID, Availability_Pct, Utilization_Pct, MTBF, MTTR
  - [ ] `Catalog_Causes` – CauseCode, Category, Description, StandardFix

#### 1.2 XLS Parser Implementation
- [ ] Define parser profiles for each tester family:
  - [ ] J750 – map Camstar columns: `TesterID`, `StartTime`, `EndTime`, `State`, `ReasonCode`
  - [ ] FLEX – handle multi-sheet XLS, extract `EquipmentState` tab
  - [ ] T2000 – parse timestamp format `MM/DD/YYYY HH:MM:SS`
  - [ ] ETS364 – handle semicolon-delimited variant
- [ ] Build validation rules:
  - [ ] Check for overlapping time spans (flag for review)
  - [ ] Validate E10 state values against allowed set
  - [ ] Reject files with >20% unmapped rows
- [ ] Create Power Automate flow: `RawLogs/` upload → trigger parser → write to Lists

#### 1.3 Triage Queue Build
- [ ] Design queue view with filters:
  - [ ] Sort by UDT_Minutes (descending)
  - [ ] Group by ToolID
  - [ ] Filter by date range (default: last 7 days)
- [ ] Implement ranking algorithm:
  - [ ] Score = (UDT_Minutes × 0.5) + (RecurrenceCount × 30) + (CriticalityWeight × 20)
  - [ ] Criticality weights: Hot Lot=5, NPI=4, High Volume=3, Others=1
- [ ] Create queue UI components:
  - [ ] Incident card: ToolID, UDT duration, E10 state, quick-view button
  - [ ] Promote to Problem button (triggers Problem list creation)
  - [ ] Link Evidence button (connects to Evidence library)

#### 1.4 Teams Notifications Enablement
- [ ] Create Teams channel `TAG Webtool Alerts`
- [ ] Configure Power Automate notifications:
  - [ ] New high-UDT incident (>120 min): post to channel with card
  - [ ] Parser validation failures: DM to uploader + TAG leads
  - [ ] Problem promotion: notify assigned owner
  - [ ] Weekly digest: summary of new incidents, open Problems, pending Changes
- [ ] Set up adaptive card templates:
  - [ ] Incident alert card: ToolID, UDT, state, direct link to triage
  - [ ] Action required card: approve/reject buttons for Change requests


### **Phase 2 – Metrics**

- Implement E10 rollups (Availability, Utilization, MTBF/MTTR)  
- Add Power BI or list‑view dashboards  

### **Phase 3 – RCA & Change with DMAIC‑Lite**

- RCA workspace (5‑Whys, Fishbone)  
- Quick **RCA 1‑Pager** generator  
- Change entity gains DMAIC‑lite export  

### **Phase 4 – Infobank & Training**

- Infobank structure (OPLs)  
- Contextual surfacing in triage  
- Training for Ops/LMT on E10 states and upload SOPs

---

## 10) SharePoint Structures & Templates

### **Libraries**
- `RawLogs/` – Camstar XLS  
- `Evidence/` – photos, traces, logs  
- `Infobank/` – OPLs, playbooks  

### **Lists**
- `Incidents` – parsed downtime rows  
- `Problems` – RCA records  
- `Changes` – DMAIC‑lite change records  
- `E10_Daily` – daily tool metrics  
- `Catalog_Causes` – standardized cause codes  

### **PPT Templates**
#### RCA 1‑Pager  
- Title / Owner / Date  
- Problem Statement  
- E10 Impact (UDT, Availability, MTBF/MTTR sparkline)  
- Containment  
- Suspected Causes  
- Next Step → DMAIC‑lite  

#### OPL (1‑Pager)
- Symptom → Cause → Standard Fix  
- Checks / Controls  
- Linked Problems/Changes  

---

## 11) KPI Definitions (per SEMI E10 / E79)

- **Availability** – productive time vs total available time  
- **Utilization** – productive time vs operational time  
- **MTBF** – mean time between E10 unscheduled downtime events  
- **MTTR** – mean time to repair those events  
- **OEE** – Availability × Performance × Quality  
- **Functional Utilization** – productive / (productive + standby‑with‑WIP)

---
