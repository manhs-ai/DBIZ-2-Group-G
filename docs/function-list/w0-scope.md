# W0 — Scope

> Lesson 5 · Source: `W0_Scope`, Use Case Diagram v3, CVCA v3, Context Diagram v3.
> Every diagram element carries an ID so each row below can be traced back (UC-xx, CVCA node/flow, Flow-xx).

## 1. Use case inventory (Table 1)

17 use cases, all assigned to a feature (W0 check: "17 listed, all assigned").

| UC ID | Use case | Primary actor | Feature |
|---|---|---|---|
| UC-01 | Submit Incident Report (description, photo/video, GPS) | Citizens | F-01 |
| UC-02 | Duplicate Detection | System (duplicate check) | F-02 |
| UC-03 | Track Report Status | Citizens | F-01 |
| UC-04 | Geofenced Alert / Notification | Citizens | F-03 |
| UC-05 | Report Missing Person / Pet / Asset | Citizens | F-01 |
| UC-06 | Update Case Status (Resolved / Handed to Police) | Police; HPC & Government Agencies (authorised staff) | F-01 |
| UC-07 | AI Classification & Confidence Scoring | System (AI Service) | F-02 |
| UC-08 | Smart Routing to Responsible Agency | System (routing engine) | F-02 |
| UC-09 | Auto-route report (confidence > 0.9) | System (routing engine) | F-02 |
| UC-10 | Auto-Notify Nearest Police Station | Police | F-03 |
| UC-11 | Community Sighting Assistance | Citizens | F-01 |
| UC-12 | Generate Reports & Analytics | HPC & Government Agencies | F-04 |
| UC-13 | LGSP Cross-Verification (RAG-based) | LGSP (Government Data & Integration Systems) | F-02 |
| UC-14 | Monitor SLA Dashboard | HPC & Government Agencies | F-04 |
| UC-15 | View Audit Trail | HPC & Government Agencies | F-04 |
| UC-16 | Manage System Integration & API Access | Technology Partners | F-05 |
| UC-17 | Human Verification (if needed) | HPC & Government Agencies | F-02 |

## 2. Features (Table 2)

5 features, within the allowed range of 3–6 (W0 check: PASS). Rule: merge any feature expected to hold fewer than 3 L2 rows; a feature that no use case or CVCA flow points at does not belong in the release.

| Feature | Name (business noun) | Scope in one sentence | Primary actors | Owner | Use cases |
|---|---|---|---|---|---|
| F-01 | Incident & Case Reporting | Citizens submit, track and manage incident reports and missing person/pet/asset cases, with community help finding what is missing | Citizens | Owner A | UC-01, UC-03, UC-05, UC-06, UC-11 |
| F-02 | AI Classification & Smart Routing | The system deduplicates, classifies and cross-verifies each report, then routes it to the responsible agency — escalating to human review when confidence is low | Police, HPC & Government Agencies (LGSP as data source) | Owner B | UC-02, UC-07, UC-08, UC-09, UC-13, UC-17 |
| F-03 | Alerts & Notifications | Citizens and police receive geofenced alerts, automatic police notifications and service-update guidance about relevant incidents | Citizens, Police | Owner C | UC-04, UC-10, (non-functional) |
| F-04 | Oversight & Analytics | HPC and government agencies monitor SLA performance, audit the module's activity and generate incident reports & analytics | HPC & Government Agencies | Owner D | UC-12, UC-14, UC-15 |
| F-05 | Platform Integration & Access | Technology partners and government integration systems (LGSP) connect infrastructure, data and APIs to the Alert Module, under role-based access control | Technology Partners, Government Data & Integration Systems (LGSP) | Owner E | UC-16, (non-functional) |

Notes:
- F-03 and F-05 each list "(non-functional)" next to their use cases: some of their functions (e.g. FR-05-02 role-based authorisation, FR-05-03 deployment) belong to no single use case and are carried by the architecture and CVCA instead.
- F-05 has no transactional function points (all rows NC), so its cost is invisible to function-point counting (see [w2-cost.md](w2-cost.md)).

## 3. Won't-have list (Table 3)

Recorded here, never dropped silently. The last two rows are W4 deferrals and are **pending client confirmation**.

| Excluded from scope | How it surfaced | Reason for exclusion |
|---|---|---|
| Manual complaint classification & unclear routing | Observed in CVCA 'Before' state (unclear route / slowly response / passive response) | Replaced by automated AI classification & confidence-based routing in the 'After' state — legacy manual routing is not carried into the Alert Module |
| General iHanoi portal functionality outside the Alert Module | Context Diagram system boundary — box is 'iHanoi Alert Module', not the whole iHanoi platform | Out of scope — the Alert Module is one module integrated into the broader iHanoi platform, not a rebuild of Hanoi Portal or iHanoi's other services |
| Online payment / tax collection | CVCA financial flow 'Tax' sits between Citizens (E1) and HPC (G2) at governance level | Financial/tax flows are outside the Alert Module boundary — no payment use case appears in the Use Case Diagram |
| Guidance & service-update notifications (FR-03-03) | W1 source CTX:flow-04; EARS criterion flagged not precise; deferred in W4 | Lowest user impact and the trigger is undefined. Deferred so in-scope effort fits team capacity. Pending client confirmation. |
| Reports & analytics for HPC / government agencies (FR-04-01) | UC-12; CVCA 'Reports & analytics' O4->G2; deferred in W4 | SLA dashboard (FR-04-02) and audit trail (FR-04-03) already give HPC oversight, so report generation moves to a later release. Pending client confirmation. |

## 4. Data functions (Table 4)

ILF = data the module maintains; EIF = data it only reads. Weights from `Parameters` §5 (ILF 10, EIF 7).

| Data group | ILF / EIF | Why | Function points |
|---|---|---|---|
| Incident Report | ILF | Created and maintained by the Alert Module — description, photo/video, GPS, category, confidence score, status (SEQ:S-01 'Save alert data'; UC-01) | 10 |
| Missing Person / Pet / Asset Case | ILF | Case record with its own lifecycle, created and maintained for missing person/pet/asset reports (UC-05) | 10 |
| SLA & Routing Metrics | ILF | Routing status, SLA thresholds and dashboard metrics maintained by the module (UC-14) | 10 |
| Audit Trail | ILF | Log of every report/case state change and routing decision, maintained for compliance (UC-15) | 10 |
| Citizen Account Directory | EIF | Citizen identity/profile read from the core iHanoi platform, never written by the Alert Module (Context Diagram: Citizens & Community Users) | 7 |
| Government Agency & Routing Directory | EIF | List of specialized departments/agencies under HPC and routing rules, read only (CVCA: O1 'Specialized departments under HPC'; Context Diagram: HPC & Government Agencies) | 7 |
| LGSP Integrated Data | EIF | Cross-verification & integrated government data read from LGSP, never written by the Alert Module (CVCA: O3 'sync data / integrated data'; Context Diagram: Government Data & Integration Systems) | 7 |

**Total data-function points: 61** (4 ILF × 10 + 3 EIF × 7).

## 5. Business rules

The workbook has no separate business-rule table. The rules below are extracted from the EARS criteria in W1, the Use Case Diagram guards and the CVCA note, and each names its source. Confirm them with the client.

| ID | Rule | Source |
|---|---|---|
| BR-01 | Confidence score **> 0.9** → report is auto-routed to the specialised department/agency under HPC (including Police). | FR-02-03; UC-09; CVCA note |
| BR-02 | Confidence score **0.9 or below** → manual path: cross-check with LGSP (RAG), and if the category is not confirmed, flag for human verification before final routing via Hanoi Portal. | FR-02-03, FR-02-04, FR-02-05; UC-13, UC-17; SQ-13..16 |
| BR-03 | A likely duplicate (same location within a time window) is flagged instead of creating a new record; the citizen is shown the existing report. Location radius and time window are **not yet defined**. | FR-02-01; SQ-04…06 |
| BR-04 | Every report/case state change writes an audit entry; authorised users can view the full trail per case. | FR-04-03; FR-01-04 |
| BR-05 | Every function is permitted or denied by the role of the calling actor (citizen, police, HPC/government staff, technology partner). | FR-05-02 |
| BR-06 | The nearest police station is notified automatically when a missing person/pet/asset case is created or a routed report needs police attention. | FR-03-02; UC-10 |
| BR-07 | Only authorised users (police, HPC/government staff) may mark a case resolved or handed to police. | FR-01-04; UC-06 |
| BR-08 | A missing person/pet/asset case is a distinct record type from a standard incident report; sightings attach only to an *active* case. | FR-01-03, FR-01-05 |
| BR-09 | Report status values shown to citizens: Submitted / Routed / In Progress / Resolved. A new report is saved as **Submitted**. | FR-01-01, FR-01-02 |
| BR-10 | Technology partners must be authenticated before the module exposes integration endpoints. | FR-05-01 |
| BR-11 | Staging deploys automatically after every merge to main (deferred to Should in W4). | FR-05-03 |

**Planning thresholds** (process rules, `Parameters` §7): Must ≤ 60% of scope effort; Could ≈ 20% (contingency pool); hidden technical work ≥ 10%; one L3 row ≤ 16 h; 3–6 features; bottom-up vs function-point variance > 30% triggers a review; capacity 240 person-hours.

## 6. Conventions and resolved inconsistencies

**Actor register** (the CVCA codes are the master; the drawn use case diagram used ACT-G1 for HPC and ACT-G2 for LGSP, which collided with CVCA G1 = Hanoi People's Council and G2 = HPC):

| Actor in these files | Code | CVCA stakeholder |
|---|---|---|
| Citizens | ACT-E1 | E1 |
| HPC & Government Agencies | ACT-G2 | G2 / O1 |
| Police | ACT-P2 | O1 (part) |
| LGSP | ACT-O3 | O3 |
| Technology Partners | ACT-P1 | P1 |
| Hanoi Portal (external system, not an actor) | n/a | O2 |
| Hanoi People's Council | n/a | G1 (not an actor of the module) |

**Other decisions applied across the files**
- **Initial report status = Submitted** (matches the basic sequence flow, FR-01-01 and the status list in FR-01-02). The full sequence diagram's "Received" is superseded.
- **Duplicates:** a likely duplicate is flagged and **no new record is created**; the citizen is shown the existing report's alert ID (FR-02-01 as written in the workbook). The "confirm or cancel" choice at SQ-06 in the full sequence drawing is dropped.
- **Hanoi Portal** is an external entity on the context diagram (Flow-11).
- **Estimate variance** (−39.5% vs ±30%) is shown as a seventh threshold check, currently REVIEW ([w2-cost.md](w2-cost.md) §6, open question #6 in [w5-handover.md](w5-handover.md)).
- **System-only use cases.** UC-02, 07, 08, 09 have no external actor in the diagram; W0 lists "System (…)" as primary actor. Convention kept.
- **F-03** has 3 L2 rows only because FR-03-03 was added from a single context flow; it is now a Won't.

The source `.drawio` and `.xlsx` files were not changed; the items above need the same edits there (list in [activity-diagrams/activity-index.md](activity-diagrams/activity-index.md) §6). The activity diagrams apply the same conventions plus the routing, audit and police-receipt conventions in §2 of that file.
