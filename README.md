# UrbanMed Sync — Hospital Operations Platform

> **Clear operations, calm leadership.** Live bed occupancy, ER wait times, OR utilization and more at a glance.

UrbanMed Sync is an interactive HTML prototype of a hospital operations dashboard MVP — a queue, bed, and patient-management platform for hospital staff.

## ✨ Features

- **Role-aware navigation** — Admin, Doctor, Nurse, Receptionist, Bed Manager each see a tailored UI
- **Live dashboard** — KPI cards, queue overview, bed availability, and critical alerts
- **Patient management** — Registration (3-step form), patient profiles, visit history
- **Queue management** — Per-department queues with priority (color + label + icon)
- **Bed management** — Visual bed map, assignment workflow with confirmation
- **Admissions pipeline** — Pending → Bed Assigned → Admitted
- **Simulated real-time** — Queue/bed/admission changes via JS timers + toasts + Live indicator
- **Dark mode** — Toggle between light and dark themes

## 🗂️ Files

| File | Description |
|---|---|
| [`index.html`](index.html) | Main interactive dashboard prototype |
| [`urbanmed-landing.html`](urbanmed-landing.html) | Product landing / marketing page |
| [`urbanmed-ad-creative.html`](urbanmed-ad-creative.html) | Ad creative / promotional page |
| [`urbanmed-video-storyboard.html`](urbanmed-video-storyboard.html) | Video storyboard / demo script |
| [`urbanmed-og.svg`](urbanmed-og.svg) | Open Graph social preview image |

## 🚀 Usage

This is a **self-contained static prototype** — no build step, no backend, no dependencies to install.

```bash
# Simply open in your browser
open index.html

# Or serve locally (optional, for consistent font loading)
python3 -m http.server 8080
# then visit http://localhost:8080
```

## 👥 Demo Roles

Switch roles via the **user menu** (top-right) to explore each perspective:

| Role | Primary focus |
|---|---|
| Administrator | Hospital-wide overview, staff, settings |
| Doctor | Assigned patients, consultation, history |
| Nurse | Ward ops, beds, admissions |
| Receptionist | Registration + queue intake |
| Bed Manager | Bed availability + admissions |

## 🏥 Demo Scenarios

Six pre-seeded scenarios make the dashboard visually testable:
1. Emergency queue with critical patients
2. ICU occupancy at 90%
3. Multiple beds becoming available
4. Admission waiting for a bed
5. Doctor with a long queue
6. No patients waiting (empty state)

## ♿ Accessibility

WCAG 2.1 AA compliant — keyboard navigation, visible focus rings, body contrast ≥4.5:1, semantic HTML, labeled form fields, ≥44px touch targets.

## 📱 Responsive

- **Desktop** ≥1280px — full sidebar layout (primary)
- **Tablet** 768–1279px — adapted layout
- **Mobile** — bottom navigation, essential workflows only

## 🛠️ Tech Stack

Pure HTML / CSS / JavaScript — no frameworks, no bundler, no dependencies.
Fonts: [Inter](https://fonts.google.com/specimen/Inter) + [Instrument Serif](https://fonts.google.com/specimen/Instrument+Serif) via Google Fonts.

---

> **Note:** This is a frontend prototype. Real-time data is simulated with JavaScript timers. A production implementation would connect to a real backend with WebSocket updates.
