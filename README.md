# 🛡️ Prahari (प्रहरी)

> **The Sentinel: Commercial Visitor Registry, Hierarchical Police Intelligence Network & Emergency Dispatch System**

[![Views](https://hits.sh/github.com/notdatkunal/prahari.svg?view=today-total&style=flat-square&label=Views&color=007ec6)](https://hits.sh/github.com/notdatkunal/prahari/)
[![GitHub Stars](https://img.shields.io/github/stars/notdatkunal/prahari?style=flat-square&logo=github&color=gold)](https://github.com/notdatkunal/prahari/stargazers)
[![GitHub Forks](https://img.shields.io/github/forks/notdatkunal/prahari?style=flat-square&logo=github)](https://github.com/notdatkunal/prahari/network)
[![Commit Activity](https://img.shields.io/github/commit-activity/m/notdatkunal/prahari?style=flat-square&logo=git)](https://github.com/notdatkunal/prahari/pulse)
[![Last Commit](https://img.shields.io/github/last-commit/notdatkunal/prahari?style=flat-square)](https://github.com/notdatkunal/prahari/commits/main)
[![License](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](LICENSE)

---

## 📌 Overview

**Prahari (प्रहरी)** is an enterprise-grade civic security and law enforcement intelligence platform that bridges commercial establishments (**Hotels, Lodges, PGs, Petrol Pumps, Warehouses, and Corporate Offices**) directly with state police hierarchies and emergency response services.

It replaces antiquated paper guest registers with a **tamper-proof digital check-in system** while equipping law enforcement with **real-time criminal watchlist matching, hierarchical geographical jurisdiction controls (Geo-RBAC), and 1-click multi-agency emergency dispatch (Police, Fire, Ambulance)**.

---

## 🌟 Dual-Facing Ecosystem

```
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                                   Prahari (प्रहरी)                                      │
├───────────────────────────────────────────┬────────────────────────────────────────────┤
│     🏨 COMMERCIAL ESTABLISHMENT PORTAL    │      👮 POLICE COMMAND & INTELLIGENCE      │
│   (Hotels, Lodges, PGs, Petrol Pumps)     │    (Station SHO ➔ DSP ➔ SP ➔ DIG ➔ DGP)    │
│                                           │                                            │
│ • Digital Guest & Visitor Check-In        │ • Hierarchical Geo-RBAC Access Control     │
│ • Government ID Verification & Photo OCR  │ • Real-Time Criminal Watchlist Interceptor │
│ • Employee / Shift Verification           │ • District & Sub-Division Crime Analytics  │
│ • 1-Click SOS Triad (Police, Amb, Fire)   │ • Broadcast BOLO (Be On Lookout) to Hotels │
│ • Theft & Incident Report Filing          │ • Automated PCR Van / Beat Officer Dispatch│
└───────────────────────────────────────────┴────────────────────────────────────────────┘
```

---

## 🚀 Key Modules & Capabilities

### 🏨 1. Establishment-Facing Portal
- **Zero-Friction Digital Check-In:** Fast visitor check-in with ID scan (Aadhaar / Passport / Driving License / Voter ID) and mobile OTP verification.
- **Petrol Pump & Night Shift Tracking:** Log transient night visitors, vehicle registration numbers, and employee shifts.
- **One-Touch Emergency SOS Triad:**
  - 🚨 **Police Panic (112):** Silent emergency alert sent directly to the local police control room with GPS location and floor map.
  - 🚑 **Medical Emergency (108):** Instant ambulance dispatch with passenger count and medical triage notes.
  - 🚒 **Fire Emergency (101):** Immediate fire station alert with establishment capacity and hazardous material warnings.
- **Incident Reporting:** Fast-track filing for room thefts, property damage, or suspicious visitor behavior with uploaded CCTV snapshots.

### 👮 2. Hierarchical Police Intelligence Command Center
- **Strict Hierarchy-Based Geo-RBAC:** Officers only access data within their assigned geographical command:
  - **Beat Constable / Patrol Officer:** Specific beat sector / ward jurisdiction.
  - **Station House Officer (SHO / Inspector):** Specific Police Station jurisdiction.
  - **Deputy Superintendent of Police (DSP / ACP):** Sub-division (cluster of police stations).
  - **Superintendent of Police (SP / DCP):** Entire District jurisdiction.
  - **Inspector General (IG / Commissioner):** Police Range / Zone (multiple districts).
  - **Director General of Police (DGP):** Statewide aggregated crime grid & cross-district analytics.
- **Real-Time Criminal Watchlist Matcher:** Instant, silent red alert triggered at the police station whenever a wanted fugitive, active warrant suspect, or missing person checks into any lodge or hotel.
- **Geo-Fenced BOLO Broadcasts:** Police can broadcast BOLO notices (with suspect photos and vehicle details) to all hotels, PGs, and petrol pumps within a 10 km, 50 km, or district-wide radius.

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      Establishment Clients                  │
│  ┌─────────────────────────┐   ┌─────────────────────────┐  │
│  │ Hotel / PG Web Register │   │ Petrol Pump Mobile App  │  │
│  │ (ID OCR + Guest Intake) │   │ (Shift & Night Visitor) │  │
│  └────────────┬────────────┘   └────────────┬────────────┘  │
└───────────────┼─────────────────────────────┼───────────────┘
                │ HTTPS (TLS 1.3 + Signed JWT)│
                ▼                             ▼
┌─────────────────────────────────────────────────────────────┐
│                    Prahari Core API Gateway                 │
│                      (FastAPI / Python)                     │
│  ┌───────────────────────────────────────────────────────┐  │
│  │ Identity & Multi-Tenant Establishment Manager         │  │
│  ├───────────────────────────────────────────────────────┤  │
│  │ Real-Time Watchlist Interception Engine               │  │
│  ├───────────────────────────────────────────────────────┤  │
│  │ Hierarchical Geo-RBAC Policy Enforcer                 │  │
│  ├───────────────────────────────────────────────────────┤  │
│  │ Emergency SOS Dispatcher & Webhook Router             │  │
│  └───────────────────────────────────────────────────────┘  │
└──────────────────────────────┬──────────────────────────────┘
                               │
            ┌──────────────────┴──────────────────┐
            ▼                                     ▼
┌──────────────────────────────┐       ┌──────────────────────────────┐
│  Police Command WebSockets   │       │      Emergency Services      │
│  (Live Alerts to PCR / HQ)   │       │  • Police Control Room (112) │
│  • Beat Officer Mobile App   │       │  • Ambulance Dispatch (108)  │
│  • District SP Dashboard     │       │  • Fire Emergency (101)      │
└──────────────────────────────┘       └──────────────────────────────┘
```

---

## 📚 Documentation

- [Police Hierarchy & Geo-RBAC Authorization Model](docs/POLICE_HIERARCHY_AND_RBAC.md)
- [Emergency Dispatch & Watchlist Matching Architecture](docs/EMERGENCY_DISPATCH_AND_WATCHLIST.md)

---

## 🛠️ Tech Stack

- **Backend:** Python (FastAPI), Node.js, WebSockets, Celery / Redis Task Queue
- **Database:** PostgreSQL + PostGIS (Geospatial boundary indexing and radius queries)
- **Frontend / Dashboards:** Next.js (React 19), Tailwind CSS, Leaflet / MapLibre (GIS Heatmaps)
- **Mobile Client:** React Native (Offline-capable check-in tablet app)
- **Security:** AES-256 Encryption at Rest, Role-Based Access Control (RBAC), Immutable Cryptographic Audit Logs

---

## 📈 Repository Telemetry & Star History

<div align="center">
  <a href="https://star-history.com/#notdatkunal/prahari&Date">
    <img src="https://api.star-history.com/svg?repos=notdatkunal/prahari&type=Date" alt="Star History Chart" width="700" />
  </a>
</div>

---

## 📄 License

This project is licensed under the MIT License.
