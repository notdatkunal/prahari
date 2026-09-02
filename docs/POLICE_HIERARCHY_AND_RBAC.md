# 👮 Police Hierarchy & Geo-RBAC Authorization Model

## 1. Geographical Hierarchy Tree

In law enforcement, data access is strictly bounded by **administrative and geographical jurisdiction**. Officers must never have access to visitor logs or surveillance data outside their assigned territorial command without formal cross-district warrant escalations.

```
                           ┌────────────────────────┐
                           │      State DGP HQ      │  (Statewide Access)
                           └───────────┬────────────┘
                                       │
                                       ▼
                           ┌────────────────────────┐
                           │   Range / Zonal DIG    │  (Multi-District Zone)
                           └───────────┬────────────┘
                                       │
                                       ▼
                           ┌────────────────────────┐
                           │   District SP / DCP    │  (Full District Scope)
                           └───────────┬────────────┘
                                       │
                                       ▼
                           ┌────────────────────────┐
                           │    Circle DSP / ACP    │  (Sub-Division / 3-5 Stations)
                           └───────────┬────────────┘
                                       │
                                       ▼
                           ┌────────────────────────┐
                           │   Station SHO / Insp   │  (Police Station Area)
                           └───────────┬────────────┘
                                       │
                                       ▼
                           ┌────────────────────────┐
                           │ Beat Constable / Patrol│  (Specific Ward / Sector)
                           └────────────────────────┘
```

---

## 2. Geo-RBAC Matrix & Permissions

| Rank | Scope Filter (SQL Condition) | Permissions |
| :--- | :--- | :--- |
| **Beat Constable** | `beat_id = user.assigned_beat_id` | View active alerts in beat, receive emergency dispatch pings, verify establishment compliance. |
| **Station SHO** | `police_station_id = user.assigned_ps_id` | Full access to all hotels, PGs, and pumps in station boundary. File FIR links, issue local summons. |
| **Circle DSP** | `circle_id = user.assigned_circle_id` | Aggregated view of 3–5 stations in circle. Authorize sub-divisional search notices. |
| **District SP** | `district_id = user.assigned_district_id` | District-wide crime analytics, criminal watchlist uploads, broadcast district-wide BOLO alerts. |
| **Range DIG** | `range_id = user.assigned_range_id` | Cross-district intelligence coordination across 3–6 districts. |
| **State DGP** | `state_code = user.state_code` | Statewide crime heatmaps, policy oversight, state-wide wanted list synchronization with CCTNS. |

---

## 3. Database Schema for Jurisdiction Modeling

```sql
-- Geographical Hierarchy
CREATE TABLE police_jurisdictions (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    level VARCHAR(32) NOT NULL, -- 'BEAT', 'STATION', 'CIRCLE', 'DISTRICT', 'RANGE', 'STATE'
    name VARCHAR(128) NOT NULL,
    parent_jurisdiction_id UUID REFERENCES police_jurisdictions(id),
    boundary_polygon GEOMETRY(Polygon, 4326),
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Officer Profiles & RBAC
CREATE TABLE police_officers (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    badge_number VARCHAR(64) UNIQUE NOT NULL,
    full_name VARCHAR(128) NOT NULL,
    rank VARCHAR(32) NOT NULL, -- 'CONSTABLE', 'SHO', 'DSP', 'SP', 'DIG', 'DGP'
    jurisdiction_id UUID REFERENCES police_jurisdictions(id) NOT NULL,
    phone_number VARCHAR(16) NOT NULL,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Immutable Cryptographic Audit Log
CREATE TABLE access_audit_logs (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    officer_id UUID REFERENCES police_officers(id),
    action VARCHAR(64) NOT NULL, -- 'SEARCH_GUEST', 'VIEW_HOTEL_REGISTER', 'DISPATCH_BOLO'
    target_record_id UUID NOT NULL,
    reason_or_fir_number TEXT NOT NULL,
    ip_address INET NOT NULL,
    signature_hash VARCHAR(64) NOT NULL, -- SHA256(officer_id + timestamp + target_record_id + previous_hash)
    created_at TIMESTAMPTZ DEFAULT NOW()
);
```
