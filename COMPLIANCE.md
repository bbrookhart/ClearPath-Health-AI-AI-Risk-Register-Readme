# COMPLIANCE.md — Control Mapping Reference

**Project:** ClearPath Health AI — AI Risk Register  
**Version:** 1.0  
**Last Reviewed:** 2026-02-23  
**Classification:** Public Portfolio Documentation  

---

## Purpose

This document maps every functional feature of the ClearPath AI Risk Register
to its corresponding regulatory or framework control. It is intended to
demonstrate to auditors, compliance teams, and hiring stakeholders that
this system was built with governance by design — not added as an afterthought.

---

## 1. NIST AI Risk Management Framework (AI RMF 1.0)

The NIST AI RMF organizes AI risk management into four functions:
GOVERN, MAP, MEASURE, and MANAGE.

### GOVERN — Establish accountability and culture

| Control ID | Control Description | Implementation in This System |
|---|---|---|
| GOVERN 1.1 | Roles and responsibilities for AI risk are established | System Owner field required on every AI system entry. Role-based access (Admin vs Auditor) enforced at every route. |
| GOVERN 1.2 | Accountability for AI risk is assigned | Audit log captures the specific username responsible for every create, update, delete, and export action. |
| GOVERN 4.1 | Org teams are committed to transparency | All system entries include intended use, data types, and FDA classification — visible to all authenticated users. |

### MAP — Identify and categorize AI risk context

| Control ID | Control Description | Implementation in This System |
|---|---|---|
| MAP 1.1 | Context is established for AI system risk assessment | Add System intake form captures: name, vendor, version, department, intended use, owner, and data types processed. |
| MAP 1.5 | Organizational risk tolerance is established | Risk scoring model (1–10) with defined thresholds for Low / Medium / High / Critical categories. |
| MAP 2.1 | Scientific findings and AI system risks are understood | MITRE ATLAS threat type field classifies each system's primary adversarial threat vector. |
| MAP 5.1 | AI risk and benefits are communicated | Dashboard displays real-time KPIs: total systems, critical risk count, PHI-processing systems count. |

### MEASURE — Analyze and assess AI risk

| Control ID | Control Description | Implementation in This System |
|---|---|---|
| MEASURE 2.1 | Risk metrics are defined and tracked | Quantitative risk score (1–10) with categorical mapping (Low/Medium/High/Critical) stored per system. |
| MEASURE 2.5 | Risk assessment results are documented | PDF export generates timestamped, user-attributed compliance report suitable for audit evidence. |
| MEASURE 4.1 | Risk metrics are monitored over time | Audit log captures timestamp of all changes; dashboard shows current risk posture across portfolio. |

### MANAGE — Prioritize and respond to AI risk

| Control ID | Control Description | Implementation in This System |
|---|---|---|
| MANAGE 1.0 | Risks are prioritized and addressed | Systems list sorted by risk score (descending). Status field tracks: Under Review / Approved / Suspended / Decommissioned. |
| MANAGE 2.2 | Risk response plans are implemented | Status workflow enforces review gate before approval. Admin-only deletion with full audit trail. |

---

## 2. HIPAA Security Rule (45 CFR Part 164)

### Administrative Safeguards — §164.308

| Standard | Implementation Required | Implementation in This System |
|---|---|---|
| §164.308(a)(1) | Security management process — risk analysis | AI system risk scoring and inventory IS the risk analysis artifact for AI-related risks. |
| §164.308(a)(3) | Workforce access management | Role-based access control limits PHI-adjacent system data to authorized users only. |
| §164.308(a)(5) | Security awareness | Login page displays mandatory access warning banner per workforce training requirements. |

### Technical Safeguards — §164.312

| Standard | Implementation Required | Implementation in This System |
|---|---|---|
| §164.312(a)(1) | Access control — unique user ID | Each user has a unique username. Shared accounts not permitted by system design. |
| §164.312(a)(2)(i) | Unique user identification | User table enforces `unique=True` constraint on username field. |
| §164.312(b) | Audit controls | Immutable AuditLog table captures: timestamp, user, action, resource, IP address, and outcome for every system event. |
| §164.312(d) | Person or entity authentication | bcrypt password hashing with salt. Passwords never stored in plaintext. Failed logins logged and flashed to user. |

---

## 3. SOC 2 Type II — Trust Services Criteria

### Common Criteria (CC Series)

| Criteria | Description | Implementation in This System |
|---|---|---|
| CC3.2 | Risk assessment — identifies risk to achieving objectives | AI system inventory with risk scoring constitutes the risk assessment artifact. |
| CC6.1 | Logical access security | Flask-Login enforces authentication on all routes via `@login_required` decorator. |
| CC6.3 | Role-based access | `@admin_required` decorator restricts write operations. Viewers can read but cannot add, delete, or export. |
| CC6.7 | Restrict transmission of sensitive info | Secrets stored in environment variables, never hardcoded. `.env` excluded from version control via `.gitignore`. |
| CC7.2 | Monitor system components | AuditLog records all access events, failures, and unauthorized attempts in real time. |
| CC4.1 | Monitoring — communication and evaluation | PDF export produces timestamped audit evidence suitable for SOC 2 auditor review. |

---

## 4. MITRE ATLAS (Adversarial Threat Landscape for AI Systems)

MITRE ATLAS classifies adversarial ML threats. This system uses ATLAS
to categorize the primary threat vector for each registered AI system,
enabling threat-informed risk scoring.

| ATLAS Technique | Technique ID | When Applied in This System |
|---|---|---|
| Craft Adversarial Data | AML.T0043 | Clinical models susceptible to input perturbation (e.g. imaging AI) |
| Backdoor ML Model | AML.T0048 | Third-party vendor models where training data is not auditable |
| Data Poisoning | AML.T0005 | Models trained on EHR data that could be manipulated at source |
| Data Exfiltration via Model | AML.T0037 | Models with broad PHI access that could leak data through outputs |
| Inference API Abuse | AML.T0040 | Externally accessible model APIs without rate limiting |
| Full Model Access / Theft | AML.T0044 | Models deployed in environments with insufficient access controls |
| Model Inversion Attack | AML.T0024 | Models that could reconstruct training data from outputs |
| ML Supply Chain Compromise | AML.T0010 | Third-party or open-source model dependencies |
| Poisoned Public Dataset | AML.T0049 | Models pre-trained on public datasets before fine-tuning |
| Model Evasion | AML.T0047 | Safety-critical models where adversaries may attempt to bypass detection |
| Erode Model Integrity | AML.T0031 | Long-running models subject to gradual performance degradation |

---

## 5. FDA AI/ML Software as a Medical Device (SaMD) Guidance

Based on the FDA's 2021 AI/ML-Based SaMD Action Plan and proposed
regulatory framework for AI-enabled devices.

| Classification | Risk Level | Regulatory Pathway | Examples in System |
|---|---|---|---|
| Non-SaMD | Operational | Not regulated as device | Scheduling AI, billing AI, documentation AI |
| SaMD Class I | Low | Exempt / 510(k) | Wellness apps, low-risk administrative tools |
| SaMD Class II | Moderate | 510(k) Premarket Notification | Diagnostic support, imaging triage, clinical alerts |
| SaMD Class III | High | PMA (Premarket Approval) | Autonomous diagnosis, autonomous treatment recommendation |

**Implementation:** Every AI system entry requires FDA classification selection. Class II and
Class III systems are visually flagged in the system inventory with color-coded badges,
elevating their visibility for compliance review.

---

## 6. NIST SP 800-53 Rev 5 — Security and Privacy Controls

| Control ID | Control Name | Implementation in This System |
|---|---|---|
| AC-2 | Account Management | User accounts created by admin only. `is_active` flag enables account suspension without deletion. |
| AC-3 | Access Enforcement | Flask route decorators enforce access control on every protected endpoint. |
| AU-2 | Event Logging | AuditLog table captures login, logout, add, delete, export, and unauthorized access events. |
| AU-9 | Protection of Audit Information | AuditLog has no delete route. No user, including admin, can remove audit records through the application. |
| SC-28 | Protection of Information at Rest | Secret key and admin credentials stored in environment variables, not in source code or database plaintext. |
| SI-12 | Information Management and Retention | Audit log query limited to 200 records for UI performance; full records retained in database. |

---

## 7. NIST Cybersecurity Framework (CSF) 2.0

| Function | Category | Implementation |
|---|---|---|
| GOVERN (GV) | GV.OC — Organizational Context | Risk register establishes organizational AI asset context. |
| IDENTIFY (ID) | ID.AM — Asset Management | AI system inventory IS the AI asset management function. |
| IDENTIFY (ID) | ID.RA — Risk Assessment | Risk scoring per system with ATLAS threat classification. |
| PROTECT (PR) | PR.AA — Identity Management & Access Control | bcrypt auth, RBAC, session management, unauthorized access logging. |

---

## Audit Evidence This System Produces

For a SOC 2 or HIPAA audit, this system can provide the following evidence artifacts:

| Evidence Type | How to Produce | Relevant Audit Request |
|---|---|---|
| AI System Inventory | Systems list page / PDF export | Asset inventory, risk assessment documentation |
| Risk Assessment Records | PDF export with risk scores and categories | Risk management program evidence |
| Access Control Logs | Audit Log page (admin only) | User access review, terminated user evidence |
| Failed Login Records | Audit Log filtered by `outcome=failure` | Security monitoring evidence |
| Unauthorized Access Attempts | Audit Log filtered by `UNAUTHORIZED_ACCESS_ATTEMPT` | Intrusion detection evidence |
| Change History | Audit Log filtered by ADD/DELETE actions | Change management evidence |

---

*This compliance mapping is maintained as a living document and should be
updated whenever new features are added or framework controls are updated.*
```

---

### Step 3 — Two Files You Still Need

**`requirements.txt`** — create this now:
```
flask==3.0.3
flask-login==0.6.3
flask-sqlalchemy==3.1.1
bcrypt==4.1.3
python-dotenv==1.0.1
reportlab==4.2.0
```

**`.env.example`** — this goes in your repo (safe to commit, unlike `.env`):
```
# Copy this file to .env and fill in real values before running
# NEVER commit your actual .env file

SECRET_KEY=replace-with-64-character-random-string
ADMIN_PASSWORD=ChangeMe2024!
