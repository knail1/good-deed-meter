# Good Deed Meter – Technical Specification (spec.md)

*Version 1.0 – June 9 2025*

---

## 1. Purpose & Scope

A minimalist Android application that allows users to record visits to self‑defined religious sites (mosque, synagogue, temple, church, etc.) as “good deeds.” The app works entirely offline after installation, storing data locally and operating a low‑overhead background location monitor based on Android Geofencing. The specification defines all functional / non‑functional requirements, data models, architecture, error‑handling, and test strategy for an MVP ready for development hand‑off.

---

## 2. Functional Requirements

| Ref   | Requirement                                                                                                                                                                                                                                        |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  F‑01 | **Add Location** screen presents a single address input; on submit the app calls OpenStreetMap Nominatim (JSON, `format=json&addressdetails=1`) and shows first 5 results for confirmation.                                                        |
|  F‑02 | Saving a location stores: editable **Custom Name** (defaults to `display_name`), **display\_name**, **boundingbox** (array of 4 strings), and **search\_url** (full GET request).                                                                  |
|  F‑03 | Immediately after saving, a 300 m geofence is registered for the location (radius computed from centroid of bounding box).                                                                                                                         |
|  F‑04 | **Background Tracking Toggle** (main dashboard) enables/disables visit logging; when **on** a persistent foreground notification is shown.                                                                                                         |
|  F‑05 | When device enters a geofence while tracking is **on**, app stores a new *Visit* row with timestamp (epoch ms), increments same‑day counter, and fires a toast: “Good deed logged ✔”.                                                              |
|  F‑06 | Multiple visits per location per day are allowed and counted.                                                                                                                                                                                      |
|  F‑07 | **History View** presents a simple reverse‑chronological list: `Custom Name – YYYY‑MM‑DD HH:MM` plus daily visit count badge.                                                                                                                      |
|  F‑08 | **Dashboard** shows: dropdown (`This Week` / `This Month` / `This Year`), total visits and % of days visited in period; last visit location & timestamp; background‑tracking toggle; header text “Good Deed Meter” in orange/yellow stylised font. |
|  F‑09 | CSV export action saves `visit_log_<ISO‑date>.csv` to the public **Downloads** directory with columns: Custom Name, display\_name, boundingbox, search\_url, visit\_timestamp, visits\_per\_day.                                                   |
|  F‑10 | On first launch only, the user sees an **Onboarding** screen explaining location usage ➜ then requests `ACCESS_FINE_LOCATION` & `ACCESS_BACKGROUND_LOCATION`.                                                                                      |
|  F‑11 | No edit/delete of saved locations; no settings screen; unlimited number of locations (note Android geofence cap ≈ 100 – handled in error section).                                                                                                 |
|  F‑12 | After device reboot, tracking is *off* by default; user must reopen app and re‑enable toggle (app will silently re‑register geofences for stored locations at that moment).                                                                        |

---

## 3. Non‑Functional Requirements

* **Target OS:** Android 8 (API 26) and above, optimised for Google Pixel devices.
* **Language:** Kotlin (preferred) or Java 8.  Jetpack libraries mandatory.
* **Footprint:** ≤ 30 MB APK, background service ≤ 1 % average CPU, battery drain ≤ 2 % per 24 h under normal use.
* **Offline First:** all core features usable without network after locations are added; OSM look‑ups require connectivity.
* **Privacy:** no data leaves device; no analytics SDKs.
* **Accessibility:** complies with TalkBack (content descriptions on clickable elements).

---

## 4. Architecture Overview

```
UI Layer  ─┬─ DashboardFragment
          ├─ AddLocationFragment
          └─ HistoryFragment
Domain     ├─ VisitLogger (handles geofence callbacks)
Layer      └─ ExportManager
Data Layer ├─ SQLiteOpenHelper (schema v1)
          ├─ LocationDao  (raw SQL wrappers)
          └─ VisitDao
Background └─ GeofenceService : ForegroundService
```

* **Geofencing API** via `com.google.android.gms:play-services-location`.
* **ForegroundService** hosts GeofencingClient, posts persistent notification (`ongoing = true`).
* **ViewModel + LiveData** feed UI; Flow/Coroutines for async DB ops.
* **Dependency Injection** with Hilt (optional but recommended).

---

## 5. Data Handling & Storage

### 5.1  SQLite Schema

```sql
CREATE TABLE locations (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  custom_name TEXT NOT NULL,
  display_name TEXT NOT NULL,
  bounding_box TEXT NOT NULL,   -- JSON string of [lat1,lat2,lon1,lon2]
  search_url TEXT NOT NULL,
  lat REAL NOT NULL,
  lon REAL NOT NULL
);

CREATE TABLE visits (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  location_id INTEGER NOT NULL REFERENCES locations(id),
  visit_ts INTEGER NOT NULL,      -- epoch milliseconds
  day_visit_count INTEGER NOT NULL
);
CREATE INDEX visits_ts_idx ON visits(visit_ts);
```

*Bounding box centroid (**`lat, lon`**) is stored to avoid recalculation at runtime.*

### 5.2  CSV Export Mapping

| Column          | Source                          |
| --------------- | ------------------------------- |
| Custom Name     | locations.custom\_name          |
| Display Name    | locations.display\_name         |
| Bounding Box    | locations.bounding\_box         |
| Search URL      | locations.search\_url           |
| Visit Timestamp | visits.visit\_ts (ISO‑8601 UTC) |
| Visits Per Day  | visits.day\_visit\_count        |

---

## 6. Geofencing Logic

1. **Register** geofence per location: radius 300 m, transition type `ENTER`, expiration `NEVER_EXPIRE`.
2. **On ENTER** callback ➜ `VisitLogger` checks tracking toggle state.
3. If enabled, `VisitLogger`:

   * Calculates local date, fetches existing visit record for same `location_id` & date; increments `day_visit_count` or inserts new row.
   * Sends `Toast` ✓ and posts a local `LiveData` event.
4. **Limit handling:** if user exceeds 100 geofences, `VisitLogger` shows warning dialog “Max locations reached (100). Delete & reinstall to reset.” (requirement states no delete/edit, so this acts as hard cap).

---

## 7. Network / API Integration

| Aspect             | Detail                                                                 |
| ------------------ | ---------------------------------------------------------------------- |
| Endpoint           | `https://nominatim.openstreetmap.org/search`                           |
| Params             | `q=<address>`, `format=json`, `limit=5`, `addressdetails=1`            |
| HTTP Method        | GET                                                                    |
| Headers            | `User-Agent: GoodDeedMeter/1.0 (contact@example.com)` (per OSM policy) |
| Rate‑limit Backoff | 429 ➜ exponential backoff starting 2 s, max 32 s, 3 retries            |
| Error Handling     | Network/unexpected → snackbar “Cannot reach address service.”          |

---

## 8. UI/UX Outline

* **OnboardingActivity** ➜ permission rationale ➜ system dialogs.
* **MainActivity** hosts 3 fragments via bottom nav (Dashboard, Add, History).
* **Visual Style:** Material3 light theme, header `Good Deed Meter` styled with gradient (orange → yellow) via ShaderBrush.
* **Toast**: plain text, short duration.

---

## 9. Permissions & Compliance

| Permission                     | When Requested | Purpose                               |
| ------------------------------ | -------------- | ------------------------------------- |
| `ACCESS_FINE_LOCATION`         | first run      | precise location for geofencing       |
| `ACCESS_BACKGROUND_LOCATION`   | first run      | detect visits while app in background |
| `POST_NOTIFICATIONS` (API 33+) | first run      | foreground service & toasts           |

If any required permission is permanently denied, UI shows banner “Location tracking disabled – enable in Settings.”

---

## 10. Error‑Handling Strategy

* **Database Failures:** wrap DB ops in try/catch ➜ log via Logcat, show snackbar.
* **Geofence Unavailable:** GEOFENCE\_NOT\_AVAILABLE ➜ show snackbar, retry after 10 min.
* **Exceed Geofence Limit:** present non‑dismissable dialog (see §6).
* **Permission Revoked Post‑Onboarding:** tracking toggle auto‑disables; banner prompts to re‑grant.
* **CSV Write Failure:** (e.g., storage full) ➜ toast “Export failed: storage unavailable.”

---

## 11. Testing Plan

### 11.1  Unit Tests (JUnit + Robolectric)

| Module      | Tests                                |
| ----------- | ------------------------------------ |
| LocationDao | CRUD, centroid calc accuracy         |
| VisitLogger | daily counter logic, date boundaries |
| CsvExporter | correct header & row mapping         |

### 11.2  Instrumentation Tests (Espresso / AndroidX Test)

1. **Onboarding Flow:** mock permissions granted, verify nav → Dashboard.
2. **Add Location:** inject mock OSM response (MockWebServer), save ➜ DB row exists.
3. **Geofence Trigger:** use `TestLocationUtil` to send enter‑event, assert toast displayed & DB insert.
4. **Export CSV:** click export, verify file exists in Downloads with correct size.

### 11.3  Manual QA Checklist

* Cold‑start ≤ 2 s.
* Tracking toggle persists across process death.
* High‑frequency geofence entries (walk in/out) do not duplicate visits beyond spec.
* Battery drain measured via Android Studio > Energy Profiler within limits.

---

## 12. Build & Deployment

* **Gradle 8+**, Android Gradle Plugin 8.2.
* Min SDK 26, Target SDK 34.
* **Proguard/R8** enabled with default rules; exclude OSM response classes.
* Release signing via Play App Signing.

---

## 13. Future Enhancements (Non‑MVP)

* Dark theme.
* Location editing/deleting and geofence recycling.
* Cloud backup / cross‑device sync.
* Widget showing weekly progress.

---
