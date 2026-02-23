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
