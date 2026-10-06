# UrbanMed Sync — Frontend Design Plan

## Intent summary

Build an interactive HTML prototype of the **UrbanMed Sync** hospital operations dashboard MVP. The product is a queue, bed, and patient-management platform for hospital staff. The prototype must feel like a real hospital operations product, not a student CRUD project — information-dense, role-aware, and fast to operate.

Core promise: **a staff member understands the current hospital situation within 10 seconds and completes their primary task within 30 seconds.**

Design hierarchy everywhere: **clarity → priority → action.**

## Deliverable format

- Responsive web prototype implemented as HTML/CSS/JS (Open Design delivers HTML, not the React/Vite codebase the PRD describes for the real product).
- Working interactions: role-aware nav, tabs, drawers, modals, filters, tables, and simulated real-time updates.
- Demo data seeded: 50–100 patients, 6 departments, 100+ beds, ~20 doctors, ~30 nurses.
- No backend in the prototype — "real-time" queue/bed/admission changes simulated with JS timers plus toasts, with a visible **Live / Connecting / Connection lost** indicator.

## Users & roles

| Role | Primary job | Nav scope | Must not see |
|---|---|---|---|
| Administrator | Hospital-wide overview, staff, settings | Everything | — |
| Doctor | Consult assigned patients | Dashboard, Patients, Queue, Consultation, History | Beds allocation, Staff, Settings |
| Nurse | Ward ops, beds, admissions | Dashboard, Patients, Beds, Admissions | Staff, Analytics |
| Receptionist | Registration + queue intake | Dashboard, Register, Patients, Queue | Beds allocation, Staff, Analytics |
| Bed Manager | Bed availability + admissions | Dashboard, Beds, Admissions | Consultation, Staff |

Rule: hide what the role can't use (backend still enforces real authz). Dashboard adapts per role — admin sees hospital-wide, doctor sees "my patients", nurse sees ward, receptionist sees intake queue, bed manager sees beds + admissions.

## MVP screen map

All screens render through one shell: **TopBar (logo, global search `Ctrl+K`, notifications, user menu) + LeftNav (role-filtered) + content area + status bar (Live indicator)**.

| Screen | Purpose | Key elements |
|---|---|---|
| Login | Auth | Email, password, remember me, forgot password |
| Dashboard | Role-adaptive overview | KPI cards, queue overview, bed availability, critical alerts |
| Patients list | Find + triage | Search, filters, sortable table |
| Patient registration | Intake | 3-step form (personal → visit → confirm) |
| Patient profile | Info workspace | Tabs: Overview / Visits / Queue history / Admission |
| Queue | Department queues | Per-department table, priority column, call actions |
| Bed dashboard | Bed KPIs + filters | Total/Available/Occupied/Maintenance/Reserved |
| Bed map | Visual grid | Clickable bed cards per ward, status colors |
| Bed assignment | Allocation workflow | Patient → department → compatible beds → confirm |
| Admissions | Request list + detail | Pending → Bed Assigned pipeline |
| Departments | Operational overview | Queue, occupancy, staff per department |
| Notifications | Alert center | Critical / Action / Info severity |
| Settings | Profile | Basic profile + role indicator |

## Key workflows (must be demonstrable end-to-end)

1. **Register patient** — Receptionist → 3-step form → confirm → success screen with Patient ID, department, queue position, estimated wait → actions: View Patient / Back to Queue.
2. **Queue → consultation** — Waiting → Call → In Consultation → profile → consultation form (reason, notes, diagnosis, treatment) → Save → Completed. Each transition visually communicated (badge change + toast).
3. **Admission → bed** — Request → Pending → find available bed → confirm assignment (prevent accidental: confirm modal shows patient/bed/department) → Bed Occupied → Admission "Bed Assigned" → Admitted.
4. **Emergency patient** — Bypasses queue order; unmistakable red priority card (color + label + icon), never subtle.

## Dashboard composition (admin reference)

- **KPI row:** Total patients (1,284 · +8.2% today), Waiting (87 · 12 urgent), Available beds (42 of 180), Occupancy (76% · +3.1%).
- **Queue overview:** department rows with waiting count + overload indicator (Emergency 18 red, Cardiology 12 amber, etc.).
- **Bed availability:** progress indicators per ward (ICU 4/20, General 18/80, …).
- **Critical alerts panel:** each alert severity-coded with an action (assign bed, call patient, view queue).
- Real data phrasing, not bare numbers: "87 patients waiting · 12 high priority · Emergency currently overloaded → View Queue".

## Component system

- Layout: AppShell, Sidebar, TopBar, PageHeader.
- Data: DataTable (sort/filter/search/pagination), StatCard, StatusBadge, PriorityBadge, ProgressBar, ChartCard (line/bar/donut/area — filled encoding, labels, tooltips, empty states).
- Forms: Input, Select, Combobox, DatePicker, Textarea, Radio (step form).
- Feedback: Toast, Alert, Modal, Drawer, Skeleton, EmptyState, ErrorState, Connection indicator.
- Healthcare: PatientCard, QueueItem, BedCard/BedMap, AdmissionCard, DepartmentCard.
- Every interactive component defines default/hover/focus/active/disabled/loading states; visible `:focus-visible` ring.

## Visual system

**Base (from Product design memory — the explicit PRD extends it):**
- Neutral tokens: background #ffffff, surface #f7f8fa, foreground #111111, muted #6b7280, border #d9dee7, accent #1677ff.
- Type: Inter everywhere (fallbacks: system-ui, -apple-system, Segoe UI, Helvetica Neue, Arial, sans-serif) — display and body may share Inter for this dense, utilitarian product.
- Layout: 8px corner radius, 1px borders, 8px baseline grid.

**Semantic status tokens (added per PRD §6 — red is used ONLY for attention):**
- Success / Available: green — `#16a34a`
- Warning / overloaded: amber — `#d97706`
- Critical / emergency: red — `#dc2626`
- Info: cyan — `#0891b2`

**Rules:**
- Priority is encoded as **color + label + icon** — never color alone.
- Accent discipline: blue #1677ff used sparingly (primary CTA, active nav, links) — not as a wash.
- No excessive gradients, no overly rounded cards, no decorative animation, no illustrations.

**Type scale (workstation-friendly, lower-res monitors):**
- Page title 28–32 · Section 20–24 · Card 16–18 · Body 14–16 · Secondary 12–14 · Labels 12–13.
- ALL-CAPS labels get ≥0.06em tracking; headings ≥32px get −0.01–0.02em.

**Dark mode:** optional toggle, deep charcoal background (not pure black), high-contrast semantic colors, readable tables/forms. Include in MVP only if trivial; otherwise defer to Phase 2.

## Interaction & state rules

- Inline actions for frequent operations (Call, Assign, Approve). Confirmation modal for irreversible ops (bed assign, discharge, delete).
- Toast for lightweight success (auto-dismiss); severity model: **critical → immediate visual attention; warning → badge + center; info → center; success → toast.**
- Preserve filters when navigating back; never clear form data on accidental nav.
- Audit-friendly: show actor + timestamp on priority changes, assignments, discharges.
- Disable duplicate submissions on action buttons ("Assigning…").

## Empty / loading / error states

- Empty dashboard → welcome state: configure Departments / Beds / Staff.
- Skeleton loaders, never blank screens.
- Human error copy + Try Again (never "Error 500").
- Connection indicator: ● Live / ● Connecting / ● Connection lost — reconnecting.

## Responsive behavior

- Desktop ≥1280 primary. Tablet 768–1279. Mobile is a redesign, not a squeeze: bottom nav (Home | Patients | Queue | Beds | More), essential workflows only, no horizontal scroll.

## Accessibility

WCAG 2.1 AA: keyboard navigation, visible focus, body contrast ≥4.5:1, semantic HTML, labeled form fields, screen-reader-friendly status indicators, no color-only status, ≥44px touch targets, clear error messages.

## Demo data & seeded scenarios

Seed 6 scenarios so the dashboard is visually testable:
1. Emergency queue contains critical patients
2. ICU occupancy at 90%
3. Multiple beds become available
4. Admission waiting for a bed
5. Doctor with a long queue
6. No patients waiting (empty state)

## Acceptance checks (Definition of Done, distilled)

- [ yes] Login screen works; role selection visible for demo (role-aware nav per §41)
- [take assption here ] Dashboard answers: how many handled / how long waiting / beds available / overloaded departments / emergencies / what needs attention
- [ yes] Patient list: search + filters + sortable table
- [ yes] Registration: 3-step form → success screen with queue position + estimated wait
- [ ] Patient profile: tabs (Overview / Visits / Queue history / Admission), status + priority prominent
- [ ] Queue: per-department view, priority always color+label+icon, call/start/prioritize actions
- [ ] Real-time updates represented: queue/bed/admission change via timers + toasts + Live indicator
- [ ] Bed dashboard + map (clickable beds), bed drawer (assign/reserve/maintenance)
- [ ] Bed assignment workflow with confirmation (prevents accidental assignment)
- [ ] Admission list + detail pipeline (Pending → Approved → Bed Assigned → Admitted)
- [ ] Departments overview with queue + occupancy
- [ ] Notifications center (Critical / Action / Info) + unread badge
- [ ] Loading skeletons, empty states, human error states present
- [ ] Keyboard accessible; focus-visible states; ≥44px targets
- [ ] Critical workflows have confirmation states
- [ ] Responsive: desktop + tablet + mobile bottom-nav redesign, no horizontal scroll
- [ ] No major screen contains placeholder UI or filler copy

## Open questions / TODOs

- [ ] Role switching for demo — add a role-picker in the user menu (fastest way to review all five role UIs), or lock to Admin?
- [ ] Dark mode in MVP or defer to Phase 2?
- [ ] Approve the 6 seeded demo scenarios above.
- [ ] Single self-contained `index.html` prototype vs. a few screen files behind a launcher — I recommend one self-contained `index.html` for the prototype.
- [ ] Confirm demo narrative names/timings are fine as invented-but-clearly-labeled sample data.

## Next step

1. Review and edit this document — answer the open questions, adjust scope or seeded scenarios.
2. Reply "approved" (or note edits).
3. Design mode builds the interactive HTML prototype from this plan.