# 🛡️ ClearPath Health AI — AI Risk Register

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python)
![Flask](https://img.shields.io/badge/Flask-3.0-lightgrey?logo=flask)
![HIPAA](https://img.shields.io/badge/HIPAA-Aligned-green)
![NIST AI RMF](https://img.shields.io/badge/NIST%20AI%20RMF-1.0-blue)
![SOC 2](https://img.shields.io/badge/SOC%202-Type%20II%20Mapped-purple)
![MITRE ATLAS](https://img.shields.io/badge/MITRE-ATLAS%20v2-red)
![License](https://img.shields.io/badge/License-MIT-yellow)

> A production-inspired, HIPAA-aligned AI governance platform for tracking, assessing,
> and managing artificial intelligence systems deployed in clinical healthcare environments.
> Built to demonstrate practical implementation of the NIST AI Risk Management Framework,
> MITRE ATLAS threat classification, FDA AI/ML SaMD guidance, and SOC 2 controls.

**[🔴 Live Demo](https://your-live-url.render.com)** &nbsp;|&nbsp;
**[📄 Compliance Mapping](./COMPLIANCE.md)** &nbsp;|&nbsp;
**[🏗️ Architecture](#system-architecture)**

---

## 🎯 Problem Statement

Healthcare organizations are deploying AI systems at unprecedented speed — sepsis
prediction models, ambient documentation tools, diagnostic imaging AI — often without
a structured governance process. Regulators including HHS, FDA, and OCR are actively
increasing scrutiny of AI in clinical settings.

This project simulates the core governance infrastructure of a Chief AI Officer, CISO,
or Compliance team would need to maintain visibility, accountability, and audit
readiness across their AI portfolio.

---

## 🏗️ System Architecture
```
┌─────────────────────────────────────────────────────────────┐
│                    ClearPath Health AI                       │
│                    AI Risk Register v1.0                     │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   ┌──────────┐    ┌──────────────┐    ┌─────────────────┐  │
│   │  Flask   │───▶│  SQLite DB   │───▶│   Audit Logger  │  │
│   │  App     │    │  (3 tables)  │    │   (immutable)   │  │
│   └──────────┘    └──────────────┘    └─────────────────┘  │
│        │                                       │            │
│   ┌──────────┐    ┌──────────────┐    ┌────────────────┐   │
│   │ RBAC     │    │  Risk Engine │    │  PDF Report    │   │
│   │ Auth     │    │  Scoring     │    │  Generator     │   │
│   └──────────┘    └──────────────┘    └────────────────┘   │
│                                                             │
└─────────────────────────────────────────────────────────────┘

Request Flow:
Browser → Flask Route → RBAC Check → Business Logic
→ Audit Log Entry → DB Write → Template Response
```

---

## ✅ Compliance Framework Coverage

| Framework | Version | Functions / Controls Implemented |
|---|---|---|
| **NIST AI RMF** | 1.0 | GOVERN 1.1, MAP 1.1, MAP 5.1, MEASURE 2.1, MEASURE 2.5, MANAGE 1.0 |
| **HIPAA Security Rule** | Current | §164.308(a)(1), §164.308(a)(5), §164.312(a)(1), §164.312(a)(2)(i), §164.312(b), §164.312(d) |
| **SOC 2 Type II** | 2017 TSC | CC3.2, CC6.1, CC6.3, CC6.7, CC7.2, CC4.1 |
| **MITRE ATLAS** | v2 | AML.T0005, AML.T0010, AML.T0024, AML.T0037, AML.T0040, AML.T0043, AML.T0044, AML.T0047, AML.T0048, AML.T0049 |
| **FDA AI/ML Guidance** | 2021 Action Plan | SaMD Classification (Class I / II / III), Intended Use Documentation |
| **NIST SP 800-53** | Rev 5 | AC-2, AC-3, AU-2, AU-9, SC-28, SI-12 |
| **NIST CSF** | 2.0 | GV.OC, ID.AM, ID.RA, PR.AA |

---

## 🔐 Security Controls Implemented

| Control | Implementation | Framework Reference |
|---|---|---|
| Password hashing | bcrypt with salt rounds | HIPAA §164.312(d), NIST AC-3 |
| Role-based access control | Admin / Viewer roles, decorator-enforced | HIPAA §164.312(a)(1), SOC 2 CC6.3 |
| Immutable audit logging | Every action logged with user, IP, timestamp | HIPAA §164.312(b), SOC 2 CC7.2 |
| Secret management | Environment variables via python-dotenv | SOC 2 CC6.7, NIST SC-28 |
| Session security | HTTPOnly cookies, SameSite=Lax | OWASP Session Management |
| Unauthorized access logging | 403 attempts captured in audit trail | NIST AU-2, SOC 2 CC7.2 |
| PDF audit evidence export | Timestamped, user-attributed compliance reports | SOC 2 CC4.1 |

---

## 🤖 AI System Risk Scoring Model

Risk scores (1–10) are assigned based on the following NIST AI RMF MEASURE 2.1
criteria assessed during system intake:

| Score Range | Category | Criteria |
|---|---|---|
| 9–10 | 🔴 Critical | Autonomous clinical decision-making, PHI processing, SaMD Class III |
| 7–8 | 🟠 High | Clinical decision support, PHI access, SaMD Class II |
| 4–6 | 🟡 Medium | Administrative AI with PHI access, or clinical AI with de-identified data |
| 1–3 | 🟢 Low | Operational/scheduling AI, no direct patient safety impact |

---

## 🚀 Getting Started

### Prerequisites
- Python 3.9+
- pip

### Installation
```bash
git clone https://github.com/YOUR-USERNAME/clearpath-ai-risk-register.git
cd clearpath-ai-risk-register

python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

pip install -r requirements.txt

cp .env.example .env
# Edit .env and set a strong SECRET_KEY before running
```

### Run Locally
```bash
python app.py
```

Open `http://localhost:5000`

| Account | Username | Password | Role |
|---|---|---|---|
| Administrator | `admin` | `ChangeMe2024!` | Full access |
| Auditor (read-only) | `auditor` | `ViewOnly2024!` | View + audit log |

### Load Demo Data
```bash
python seed_data.py
```

---

## 📁 Project Structure
```
clearpath-ai-risk-register/
├── app.py                  # Main Flask application + routes
├── models.py               # SQLAlchemy database models
├── reports.py              # PDF report generation (ReportLab)
├── seed_data.py            # Demo data loader
├── requirements.txt        # Python dependencies
├── .env.example            # Environment variable template
├── .gitignore              # Excludes secrets, venv, DB files
├── README.md               # This file
├── COMPLIANCE.md           # Full framework control mapping
└── templates/
    ├── base.html           # Shared layout with navigation
    ├── login.html          # Authenticated entry point
    ├── dashboard.html      # KPI overview + recent audit activity
    ├── systems.html        # AI system inventory table
    ├── add_system.html     # Intake form (MAP 1.1)
    └── audit_log.html      # Immutable event log viewer
```

---

## 🗺️ Roadmap — ClearPath Health AI Suite

This is Project 1 of a 6-project portfolio demonstrating end-to-end AI GRC capability:

| # | Project | Status | Frameworks |
|---|---|---|---|
| 1 | **AI Risk Register** ← You are here | ✅ Complete | NIST AI RMF, HIPAA, SOC 2, ATLAS, FDA |
| 2 | Bias & Fairness Audit Pipeline | 🔄 In Progress | NIST AI RMF MEASURE, FDA, IEEE 7010 |
| 3 | AI Incident Response System | 🔜 Planned | NIST RMF 2.0, HIPAA Breach Rule, ATLAS |
| 4 | Compliance Control Dashboard | 🔜 Planned | SOC 2 TSC, HIPAA, NIST CSF 2.0 |
| 5 | Model Card Transparency Portal | 🔜 Planned | FDA SaMD, EU AI Act, NIST AI RMF |
| 6 | ATLAS Threat Modeling Tool | 🔜 Planned | MITRE ATLAS, STRIDE, NIST AI RMF MAP |

---

## ⚠️ Disclaimer

This project is a portfolio demonstration built for educational and career development
purposes. It simulates governance infrastructure for a fictional healthcare organization
("ClearPath Health"). It does not process real patient data and should not be deployed
in a production clinical environment without a formal security review, penetration test,
and compliance assessment.

---

## 👤 Author

Built by Brian Brookhart as part of an AI GRC portfolio targeting roles in:
- AI Risk & Compliance
- Healthcare Technology Governance  
- Responsible AI / Trustworthy AI
- Information Security GRC

**Connect:** https://www.linkedin.com/brian-brookhart

## 🚀 Run
python app.py - Open your browser to http://localhost:5000 and log in with admin / ChangeMe2026!
