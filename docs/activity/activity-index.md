# Activity diagram index

> Lesson 4 · 17 activity diagrams, one per use case. Generated from the files in this folder on 2026-11-07. Every diagram is a plain Mermaid `flowchart` with one subgraph per lane and was syntax-checked with Mermaid 12.3.0 (visual render on mermaid.live not run).

## 1. Index

| AD | Use case | Feature | Priority (W4) | Lanes | Open questions | Blocking |
|---|---|---|---|---|---|---|
| [AD-UC-01](AD-UC-01.md) | UC-01 Submit Incident Report | F-01 | Must | Citizens & Community Users · iHanoi Alert Module | 2 | 0 |
| [AD-UC-02](AD-UC-02.md) | UC-02 Duplicate Detection | F-02 | Should | iHanoi Alert Module | 3 | 1 |
| [AD-UC-03](AD-UC-03.md) | UC-03 Track Report Status | F-01 | Should | Citizens & Community Users · iHanoi Alert Module | 2 | 0 |
| [AD-UC-04](AD-UC-04.md) | UC-04 Geofenced Alert / Notification | F-03 | Should | Citizens & Community Users · iHanoi Alert Module | 3 | 0 |
| [AD-UC-05](AD-UC-05.md) | UC-05 Report Missing Person / Pet / Asset | F-01 | Must | Citizens & Community Users · iHanoi Alert Module | 5 | 1 |
| [AD-UC-06](AD-UC-06.md) | UC-06 Update Case Status | F-01 | Should | Police · HPC & Government Agencies · iHanoi Alert Module | 5 | 0 |
| [AD-UC-07](AD-UC-07.md) | UC-07 AI Classification & Confidence Scoring | F-02 | Must | iHanoi Alert Module · AI Service | 2 | 1 |
| [AD-UC-08](AD-UC-08.md) | UC-08 Smart Routing to Responsible Agency | F-02 | Must | iHanoi Alert Module · Hanoi Portal | 4 | 2 |
| [AD-UC-09](AD-UC-09.md) | UC-09 Auto-route report | F-02 | Must | iHanoi Alert Module · HPC & Government Agencies | 2 | 0 |
| [AD-UC-10](AD-UC-10.md) | UC-10 Auto-Notify Nearest Police Station | F-03 | Must | iHanoi Alert Module · Police | 5 | 0 |
| [AD-UC-11](AD-UC-11.md) | UC-11 Community Sighting Assistance | F-01 | Could | Citizens & Community Users · iHanoi Alert Module | 5 | 0 |
| [AD-UC-12](AD-UC-12.md) | UC-12 Generate Reports & Analytics | F-04 | Won't | HPC & Government Agencies · iHanoi Alert Module | 3 | 0 |
| [AD-UC-13](AD-UC-13.md) | UC-13 LGSP Cross-Verification | F-02 | Could | iHanoi Alert Module · Government Data & Integration Systems | 2 | 1 |
| [AD-UC-14](AD-UC-14.md) | UC-14 Monitor SLA Dashboard | F-04 | Could | HPC & Government Agencies · iHanoi Alert Module | 5 | 0 |
| [AD-UC-15](AD-UC-15.md) | UC-15 View Audit Trail | F-04 | Must | HPC & Government Agencies · iHanoi Alert Module | 4 | 0 |
| [AD-UC-16](AD-UC-16.md) | UC-16 Manage System Integration & API Access | F-05 | Must | Technology Partners · iHanoi Alert Module | 5 | 2 |
| [AD-UC-17](AD-UC-17.md) | UC-17 Human Verification | F-02 | Must | HPC & Government Agencies · iHanoi Alert Module | 5 | 4 |

Totals: 62 open questions, 12 of them blocking, in 7 of the 17 diagrams (F-01: 1, F-02: 9, F-05: 2). The three blocking questions of [w5-handover.md](../w5-handover.md) (#1 to #3) overlap with AD-UC-17 questions 5 and 2 and with the LGSP read mode (ADR-004); the rest are new.

## 2. Conventions applied in every diagram

| # | Convention | Where decided |
|---|---|---|
| C1 | Actor codes follow the CVCA: Citizens ACT-E1, HPC & Government Agencies ACT-G2, Police ACT-P2, LGSP ACT-O3, Technology Partners ACT-P1 | [w0-scope.md](../w0-scope.md) §6 |
| C2 | Lane names are the Context Diagram entity names: "Citizens & Community Users", "HPC & Government Agencies", "Police", "Technology Partners", "Government Data & Integration Systems (LGSP)", plus "iHanoi Alert Module". Two exceptions are lanes for parts that are not Context entities: "AI Service (Classification)" (ADR-003) and "Hanoi Portal" (Context Flow-11, added) | [context-diagram.md](../context-diagram.md) |
| C3 | A new report is saved as **Submitted**. Routing sets **Routed** on both paths (auto and manual). A report flagged for human verification stays Submitted | FR-01-02, BR-09 |
| C4 | A likely duplicate is flagged and **no new record is created**; the citizen is shown the existing alertId. There is no confirm or cancel step | FR-02-01, BR-03 |
| C5 | Classification (UC-07) runs once per report: UC-01 calls UC-08, UC-08 calls UC-07 | Use Case Diagram |
| C6 | UC-08 owns the final routing. UC-09 is the auto-route branch (called by UC-08, starts UC-04). UC-13 and UC-17 only return a result to UC-08 | Use Case Diagram; SQ-16 |
| C7 | Every state transition after creation shows one call step "Record audit entry (AD-UC-15)": AD-UC-06, AD-UC-08, AD-UC-09. Setting the first status Submitted, and flagging for verification, are not transitions | FR-04-03, BR-04 |
| C8 | The police receive a notification in AD-UC-10 only; AD-UC-05 calls it | FR-03-02 |
| C9 | The push provider is part of the internal Notification Service (no lane, no Context entity) | SQ-12; FR-03-01-INT |
| C10 | LGSP data is synchronised as a daily batch by default | ADR-004 |
| C11 | The 0.9 threshold is decided once, in AD-UC-08 (d1); AD-UC-09 starts after it | FR-02-03 |

## 3. Call map

| Diagram | Calls | Returns to / called by |
|---|---|---|
| AD-UC-01 | AD-UC-02 (duplicate check), AD-UC-08 (classify and route) | Starts the flow |
| AD-UC-05 | AD-UC-10, AD-UC-11 | Starts the flow |
| AD-UC-06 | AD-UC-15 diagram B (audit) | Starts the flow |
| AD-UC-08 | AD-UC-07, AD-UC-09, AD-UC-13, AD-UC-15 diagram B | Called by AD-UC-01; receives results from AD-UC-13 and AD-UC-17 |
| AD-UC-09 | AD-UC-04, AD-UC-15 diagram B | Called by AD-UC-08 |
| AD-UC-13 | AD-UC-17 | Called by AD-UC-08; returns to AD-UC-08 |
| AD-UC-17 | none | Called by AD-UC-13; returns to AD-UC-08 |
| AD-UC-02, 03, 04, 07, 10, 11, 12, 14, 15, 16 | none | Called as shown above or started by an actor |

## 4. State table

| Object | State | Set by | Notes |
|---|---|---|---|
| Incident Report | Submitted | AD-UC-01 (s5) | Also the state while flagged for human verification |
| Incident Report | Routed | AD-UC-08 (s2), AD-UC-09 (s2) | Same state on the auto and manual paths |
| Incident Report | In Progress | none | Open: no function sets it (AD-UC-03 question 2) |
| Incident Report / Case | Resolved | AD-UC-06 | FR-01-04 |
| Incident Report / Case | Handed to Police | AD-UC-06 | **Proposed fifth status**; FR-01-02 lists four. Citizens see it as In Progress until decided |
| Missing Person / Pet / Asset Case | initial status | AD-UC-05 | Not defined (AD-UC-05 question 2) |
| Sighting | Linked to Case | AD-UC-11 (o1) | Descriptive |
| Analytics Report | Generated | AD-UC-12 (o1) | Descriptive; deferred (Won't) |
| Audit entry | Recorded | AD-UC-15 diagram B | Descriptive |

## 5. Blocking questions

| AD | # | Blocking question | Feature |
|---|---|---|---|
| AD-UC-02 | 1 | what are the location radius and the time window? FR-02-01 gives no values and W4 asks to fix them before build | F-02 |
| AD-UC-05 | 1 | is the declaration rejected when description or last-known location is missing, and which message does the citizen get? | F-01 |
| AD-UC-07 | 1 | what happens if the AI Service returns no result or an error (retry, manual path, wait time)? | F-02 |
| AD-UC-08 | 1 | what happens when no specialized agency matches the category? W1 FR-02-03-INT covers the agency directory but gives no rule for a missing match | F-02 |
| AD-UC-08 | 2 | what happens after manual classification at Hanoi Portal? Is the result sent back to the module? | F-02 |
| AD-UC-13 | 2 | does the release include FR-02-04 (Could), which supplies the result needed by FR-02-05 (Must)? | F-02 |
| AD-UC-16 | 1 | when partner authentication fails, what does the module do (reject only, lock out after repeated failures, write an audit entry per FR-04-03)? | F-05 |
| AD-UC-16 | 5 | FR-05-01 names "integration endpoints defined for the Alert Module" but W1 does not list them. Which endpoints exist, and which role may use which? | F-05 |
| AD-UC-17 | 1 | when the reviewer does not verify the category, what happens (return to queue, reject the report, ask the citizen, other)? | F-02 |
| AD-UC-17 | 2 | who are the human reviewers (which HPC staff roles), and do they need a reviewer screen? FR-02-05 has a BE row only (W5 question 3) | F-02 |
| AD-UC-17 | 4 | W4 demoted FR-02-04 (LGSP cross-check) to Could, but FR-02-05 (Must) depends on its result. What handles low-confidence reports if FR-02-04 is not built? | F-02 |
| AD-UC-17 | 5 | source of the 0.9 threshold, policy value or model default (W5 question 1) | F-02 |

Resolve the F-02 questions before the first F-02 sprint. AD-UC-13 question 2, AD-UC-17 question 4 and AD-UC-08 question 4 are one issue seen from three diagrams: FR-02-05 (Must) depends on the LGSP result of FR-02-04 (Could).

## 6. Edits still needed in the source drawings

| Drawing | Edit |
|---|---|
| Use Case Diagram | Add the line UC-01 to UC-08; relabel ACT-G1 to ACT-G2 (HPC) and ACT-G2 to ACT-O3 (LGSP); mark UC-12 as out of release |
| Context Diagram | Add Hanoi Portal and Flow-11; rename "Police (UC-P2)" to "Police" with code ACT-P2 |
| Sequence Diagram v3 | Initial status "Submitted" at SQ-07; drop the confirm/cancel choice at SQ-06; show SQ-17 to SQ-19 in parallel with SQ-08 to SQ-16 |
| Workbook | Add open questions #6 and #7 to W5; add CTX:Flow-04 to the FR-01-02 source trace; decide the fifth status "Handed to Police" |
