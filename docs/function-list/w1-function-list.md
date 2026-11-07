# W1 — Function list

> Lesson 5 · Source: `W1_FunctionList`. Features come from [w0-scope.md](w0-scope.md); sweeps were done one diagram at a time (Use Case, Sequence, Context, CVCA, Architecture + ADR) so the source trail stays clean.
> No money on this page: cost is in [w2-cost.md](w2-cost.md).

**Totals:** 19 L2 functions · 31 L3 work items · 268 base hours · 56 transactional function points.

- **L2** = a business behaviour the client signs off. It carries the EARS criterion and the function-point type.
- **L3** = one work item in one technical layer (FE / BE / INT), sized S / M / L (= 4 / 8 / 16 h; ceiling 16 h per L3).
- **FP type:** EI = input (4), EO = output with derived data (5), EQ = inquiry (4), NC = not countable (0): business rules and non-functional work such as authorisation or deployment.
- **Initial priority** is the W1 value; the final value after review is in [w4-priorities.md](w4-priorities.md).

## 1. L2 functions (client-facing)

| L2 ID | Feature | Function | EARS acceptance criterion | Source trace | Initial priority | FP type | FP value |
|---|---|---|---|---|---|---|---|
| FR-01-01 | F-01 | Submit an incident report | WHEN a citizen submits an incident report with description, photo/video and GPS location, the system SHALL create the report and return a confirmation with alert ID and status. | UC:UC-01; SEQ:SQ-01, SQ-06, SQ-19 | Must | EI | 4 |
| FR-01-02 | F-01 | Track report status | WHEN a citizen opens a submitted report, the system SHALL display its current status (Submitted / Routed / In Progress / Resolved). | UC:UC-03; CTX:Flow-04 (added); CVCA:'Submit complaint' E1->O4 | Must | EQ | 4 |
| FR-01-03 | F-01 | Report a missing person, pet or asset | WHEN a citizen reports a missing person, pet or asset with description and last-known location, the system SHALL create a missing-person/pet/asset case, distinct from a standard incident report. | UC:UC-05 | Must | EI | 4 |
| FR-01-04 | F-01 | Update case status (resolved / handed to police) | WHEN an authorised user marks a case as resolved or handed to police, the system SHALL update the case status and record the change in the audit trail. | UC:UC-06 | Must | EI | 4 |
| FR-01-05 | F-01 | Community sighting assistance | WHEN a missing-person/pet/asset case is active, the system SHALL allow nearby citizens to submit sighting information linked to that case. | UC:UC-11 | Could | EI | 4 |
| FR-02-01 | F-02 | Detect duplicate reports | WHEN a new incident report is submitted, the system SHALL check for existing reports at the same location within a time window and SHALL flag likely duplicates instead of creating a new record. | UC:UC-02; SEQ:SQ-04, SQ-05 | Must | NC | 0 |
| FR-02-02 | F-02 | Classify report and score confidence | WHEN an incident report is received, the AI Service SHALL classify it into a category and SHALL return a confidence score with the classification. | UC:UC-07; SEQ:SQ-08, SQ-10 | Must | EI | 4 |
| FR-02-03 | F-02 | Route report to the responsible agency | WHEN a report has been classified, IF the confidence score is greater than 0.9, THEN the system SHALL auto-route it to the specialized department/agency under HPC; IF the confidence score is 0.9 or below, THEN the system SHALL route it to Hanoi Portal for manual classification. | UC:UC-08; UC:UC-09; SEQ:SQ-11, SQ-16; CVCA: classify & auto route / map & route; CTX:Flow-11 (added) | Must | EI | 4 |
| FR-02-04 | F-02 | Cross-verify report against LGSP data | WHEN a report is routed for verification, the system SHALL cross-check it against integrated government data from LGSP using a RAG-based lookup. | UC:UC-13; SEQ:SQ-13, SQ-14; CVCA:O3 'sync data / integrated data' | Should | EQ | 4 |
| FR-02-05 | F-02 | Escalate to human verification on low confidence | IF the AI confidence score is 0.9 or below AND the LGSP cross-check does not confirm the category, THEN the system SHALL flag the report for human verification before final routing. | UC:UC-17; SEQ:SQ-15 (branch confidence 0.9 or below) | Must | NC | 0 |
| FR-03-01 | F-03 | Send geofenced alert to nearby citizens | WHEN a report is routed for public attention, the system SHALL notify citizens within the affected geofenced area. | UC:UC-04; SEQ:SQ-12; CVCA: Response & alert O4->E1 | Must | EO | 5 |
| FR-03-02 | F-03 | Auto-notify the nearest police station | WHEN a missing-person/pet/asset case is created or a routed report requires police attention, the system SHALL automatically notify the nearest police station. | UC:UC-10; SEQ:SQ-12 | Must | EO | 5 |
| FR-03-03 | F-03 | Send guidance and service-update notifications | WHEN the module has guidance or a service update relevant to a citizen's report, the system SHALL send a notification with that guidance or update. | CTX:flow-04 | Could | EO | 5 |
| FR-04-01 | F-04 | Generate reports and analytics for HPC & government agencies | WHEN an authorised HPC/government user requests analytics, the system SHALL generate a report of incident volumes, categories and resolution performance. | UC:UC-12; CVCA:'Reports & analytics' O4->G2 | Should | EO | 5 |
| FR-04-02 | F-04 | Monitor SLA dashboard | WHEN an authorised HPC/government user opens the SLA dashboard, the system SHALL display report routing and resolution times against SLA thresholds. | UC:UC-14 | Should | EQ | 4 |
| FR-04-03 | F-04 | Maintain and view audit trail | WHEN any report or case changes state, the system SHALL record an audit entry, and an authorised user SHALL be able to view the full audit trail for a case. | UC:UC-15 (BE: audit written on each state change) | Must | NC | 0 |
| FR-05-01 | F-05 | Manage system integration & API access for technology partners | WHEN a technology partner requests API access, the system SHALL authenticate the request and SHALL expose the integration endpoints defined for the Alert Module. | UC:UC-16; CVCA:'Build' P1->O4; CTX:flow-05,flow-06 | Must | NC | 0 |
| FR-05-02 | F-05 | Enforce role-based authorisation | WHEN any actor (citizen, police, HPC/government staff, technology partner) invokes a function, the system SHALL permit or deny access according to the role assigned to that actor. | UC:UC-16 (cross-cutting: applies to UC-01 to UC-17); CVCA:actors G1/G2/O1/O4/P1/E1 | Must | NC | 0 |
| FR-05-03 | F-05 | Provision environment and deploy the Alert Module | The system SHALL deploy automatically to the staging environment after every merge into the main branch. | CVCA:'Build' P1->O4 | Must | NC | 0 |

## 2. L3 work items

| L3 ID | L2 | Layer | Work item | Size | Base h | EARS precise? | Hidden work? |
|---|---|---|---|---|---|---|---|
| FR-01-01-FE | FR-01-01 | FE | Screen: capture description, photo/video, GPS and submit report | M | 8 | Yes | No |
| FR-01-01-BE | FR-01-01 | BE | API: validate submission and persist incident report | M | 8 | Yes | No |
| FR-01-02-FE | FR-01-02 | FE | Screen: my reports list & status detail | S | 4 | Yes | No |
| FR-01-02-BE | FR-01-02 | BE | API: read report status | S | 4 | Yes | No |
| FR-01-03-FE | FR-01-03 | FE | Form: declare missing person/pet/asset details | M | 8 | Yes | No |
| FR-01-03-BE | FR-01-03 | BE | API: create missing-case record | M | 8 | Yes | No |
| FR-01-04-FE | FR-01-04 | FE | Screen: mark case resolved / escalate to police | S | 4 | Yes | No |
| FR-01-04-BE | FR-01-04 | BE | API: update case status and write audit entry | M | 8 | Yes | Yes |
| FR-01-05-FE | FR-01-05 | FE | Screen: submit a sighting against an active case | M | 8 | Yes | No |
| FR-01-05-BE | FR-01-05 | BE | API: link sighting to case | S | 4 | Yes | No |
| FR-02-01-BE | FR-02-01 | BE | Duplicate-matching logic (location + time window) | M | 8 | No | No |
| FR-02-02-BE | FR-02-02 | BE | API: send report to AI Service for classification | S | 4 | Yes | No |
| FR-02-02-INT | FR-02-02 | INT | Integrate AI Service classification & confidence-scoring model | L | 16 | No | No |
| FR-02-03-BE | FR-02-03 | BE | Routing engine: confidence-based routing rule (>0.9 auto / <=0.9 manual) | L | 16 | Yes | No |
| FR-02-03-INT | FR-02-03 | INT | Integrate specialized-department/agency directory (O1) and Hanoi Portal (O2) | M | 8 | No | No |
| FR-02-04-INT | FR-02-04 | INT | Integrate LGSP data sync and RAG-based cross-verification query | L | 16 | No | No |
| FR-02-05-BE | FR-02-05 | BE | Human-verification queue and reviewer workflow | M | 8 | Yes | No |
| FR-03-01-BE | FR-03-01 | BE | Geofencing & alert-targeting logic | M | 8 | Yes | No |
| FR-03-01-INT | FR-03-01 | INT | Integrate push-notification service | M | 8 | No | No |
| FR-03-02-BE | FR-03-02 | BE | Nearest-station lookup and notification trigger | M | 8 | Yes | No |
| FR-03-03-BE | FR-03-03 | BE | Guidance/service-update notification trigger | S | 4 | No | No |
| FR-04-01-FE | FR-04-01 | FE | Screen: analytics & reports dashboard | M | 8 | No | No |
| FR-04-01-BE | FR-04-01 | BE | Aggregation queries for reports & analytics | M | 8 | No | No |
| FR-04-02-FE | FR-04-02 | FE | Screen: SLA dashboard with routing/resolution timers | M | 8 | Yes | No |
| FR-04-02-BE | FR-04-02 | BE | SLA calculation & threshold-breach logic | M | 8 | Yes | No |
| FR-04-03-BE | FR-04-03 | BE | Audit-logging on every state change | M | 8 | Yes | Yes |
| FR-04-03-FE | FR-04-03 | FE | Screen: view audit trail per case | S | 4 | Yes | No |
| FR-05-01-INT | FR-05-01 | INT | API gateway & partner authentication | L | 16 | No | Yes |
| FR-05-01-BE | FR-05-01 | BE | Partner/API-access management | M | 8 | Yes | Yes |
| FR-05-02-BE | FR-05-02 | BE | Role-based access-control model & permission checks on every endpoint | L | 16 | Yes | Yes |
| FR-05-03-INT | FR-05-03 | INT | Build CI/CD pipeline and staging environment | L | 16 | Yes | Yes |

## 3. Health of the list

| Measure | Value |
|---|---|
| L3 rows by layer | BE: 17 rows / 136 h; FE: 8 rows / 52 h; INT: 6 rows / 80 h |
| Hidden technical work | 6 rows, 72 h = 26.9% of base hours (floor 10%) |
| L3 rows above the 16 h ceiling | 0 |
| L3 rows with a layer label | all 31 |
| EARS criteria flagged "not precise" | 9 L3 rows (these carry the 0.40 verification factor instead of 0.15) |
| Integrity line (W1) | All L3 rows have a numeric base-hours value |

Rows flagged *not precise*: FR-02-01-BE, FR-02-02-INT, FR-02-03-INT, FR-02-04-INT, FR-03-01-INT, FR-03-03-BE, FR-04-01-FE, FR-04-01-BE, FR-05-01-INT.

## 4. Things to know before building

- **EARS gaps to close.** FR-02-01 needs a location radius and a time window; FR-02-03 and FR-02-05 must agree on the 0.9 boundary (fixed in W3); FR-02-04 needs the LGSP read mode (daily batch vs real time).
- **Missing reviewer screen.** FR-02-05 has a BE row only: no screen for the human reviewer is costed (open question #3 in [w5-handover.md](w5-handover.md)).
- **No FR covers the gateway relay steps** SQ-03 and SQ-18 on their own; they are folded into FR-05-01-INT.
- **Rows with no use case:** FR-03-03 (context flow only) and FR-05-03 (CVCA "Build" flow only).
- **Not covered by any row:** validation-error handling for an invalid report (shown in the simple UC-01 sequence diagram, step 5a) beyond "validate submission" in FR-01-01-BE.
