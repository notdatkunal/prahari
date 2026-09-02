# 🚨 Emergency Dispatch, SOS Triad & Watchlist Matching Architecture

## 1. Real-Time Criminal Watchlist Interception

Whenever a guest checks into a commercial establishment, the Prahari engine performs an asynchronous **zero-delay background match**:

```
Guest Check-In (Aadhaar / ID / Phone / Photo)
                     │
                     ▼
       ┌───────────────────────────┐
       │   SHA-256 Hashing & OCR   │
       └─────────────┬─────────────┘
                     │
                     ▼
       ┌───────────────────────────┐
       │   Watchlist Match Engine  │
       │ • Exact ID Hash Match     │
       │ • Fuzzy Name + DOB Match  │
       │ • Facial Vector Match     │
       └─────────────┬─────────────┘
                     │
         ┌───────────┴───────────┐
         │ Match Found?          │
         ▼                       ▼
       [ NO ]                 [ YES ]
         │                       │
     Save to Log                 ▼
                   🚨 SILENT RED ALERT DISPATCH
                   • Notify Station SHO & Beat Patrol
                   • Push to District SP Control Room
                   • Hotel receptionist receives NO warning
                     (ensures suspect does not flee)
```

---

## 2. Emergency SOS Triad Dispatch Pipeline

When an establishment presses an SOS panic button:

```json
{
  "sos_event_id": "sos_884920",
  "emergency_type": "POLICE_AND_AMBULANCE",
  "establishment": {
    "id": "est_taj_residency_04",
    "name": "Hotel Taj Residency",
    "type": "HOTEL_LODGE",
    "address": "MG Road, Sector 4",
    "coordinates": {"lat": 12.9716, "lng": 77.5946},
    "manager_contact": "+91-9876543210"
  },
  "incident_details": {
    "nature": "Violent altercation / weapon spotted in Room 204",
    "reported_by": "Night Front Desk",
    "cctv_snapshot_url": "https://vault.prahari.gov.in/evidence/snap_99.jpg"
  },
  "dispatch_targets": [
    {
      "agency": "POLICE",
      "assigned_station": "Central Police Station",
      "nearest_patrol_unit": "PCR Van 08 (1.2 km away)"
    },
    {
      "agency": "AMBULANCE",
      "provider": "City General Hospital 108 Dispatch"
    }
  ]
}
```

---

## 3. Geo-Fenced BOLO (Be On the Lookout) Broadcast Protocol

1. **Trigger:** District SP or Range DIG issues a BOLO for an escaped suspect or stolen vehicle.
2. **Radius Resolution:** PostGIS query selects all active establishments within `ST_DWithin(establishment_location, incident_location, radius_meters)`.
3. **Push Dispatch:** Instant WebSocket push notification + SMS alert dispatched to all hotel front desks, security managers, and petrol pump booths within the zone.
