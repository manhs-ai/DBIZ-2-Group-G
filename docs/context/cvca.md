# CVCA — Customer Value Chain Analysis

> Lesson 3 · Source: `iHanoi_Alert_CVCA_v3.drawio` (pages "CVCA BEFORE" and "CVCA AFTER"). The diagram stores arrows without explicit endpoints, so flow directions below were read from arrow positions and cross-checked against the W1 source traces. Verify against the drawing.
> In the diagram, **blue arrows = information flows** and **red arrows = money flows**.

## 1. Stakeholders

| ID | Stakeholder | Zone | Role for the Alert Module |
|---|---|---|---|
| G1 | Hanoi People's Council | Governance | Authorises HPC |
| G2 | HPC | Governance | Directs operations, allocates budget, pays partners |
| O1 | Specialised departments under HPC (incl. Police) | Operation | Receive routed reports, act on them, report back |
| O2 | Hanoi Portal | Operation | Receives manually routed reports |
| O3 | LGSP | Operation | Source of integrated government data |
| O4 | iHanoi | Operation | Host platform; the Alert Module is its "ALERT SYSTEM" |
| P1 | Technological corporations | Partner | Build the platform |
| E1 | Hanoi citizens | End users | Submit complaints, receive responses and alerts |

## 2. Flows (after state)

| Type | From → To | Label |
|---|---|---|
| Info | G1 → G2 | authorize |
| Info | G2 → G1 | report |
| Info | G2 → Operation (O1…) | direct |
| Info | Operation → G2 | report |
| **Money** | G2 → Operation | Budget allocation |
| **Money** | G2 → P1 | Expense |
| Info | P1 → O4 and Operation | Build |
| Info | O3 ↔ operation / O4 | sync data; integrated data |
| Info | O1 ↔ O2 | map & route |
| Info | E1 → O4 | Submit complaint |
| Info | O4 → O1 | * submit; * classify & auto route |
| Info | Operation → O4 | response |
| Info | O4 → E1 | Response & alert |
| **Money** | E1 → HPC | Tax (outside module scope) |

\* Note on the diagram: confidence > 0.9 → auto-routed to specialised departments and agencies under HPC (incl. Police). Confidence 0.9 or below → cross-checked with LGSP, flagged for human verification, then routed via Hanoi Portal.

## 3. Before vs after

| Aspect | Before | After |
|---|---|---|
| Citizen input | "Submit complaint & manual classify" | "Submit complaint" |
| Routing | "unclear route", "slow response" | "classify & auto route", "map & route" |
| Citizen feedback | "Passive response" | "Response & alert" |
| Platform | iHanoi only | iHanoi + **ALERT SYSTEM** (AI classification, confidence-based routing, LGSP cross-check) |
| Money | Budget, expense, tax | Unchanged: the module adds no new money flow |

## 4. Problems

| ID | Problem (before state) | Who feels it |
|---|---|---|
| P-1 | Complaints are classified by hand | O4/O1 staff |
| P-2 | Unclear route to the responsible agency | E1, O1 |
| P-3 | Slow response | E1 |
| P-4 | Passive response: citizens receive little feedback or alert | E1 |
| P-5 | Missing-person/pet/asset cases have no dedicated flow (inferred from UC-05 and UC-10) | E1, Police |

## 5. Stakeholder requirements

Derived from the problems and flows; each is realised by the functions in [w1-function-list.md](w1-function-list.md).

| ID | Stakeholder | Requirement | Addresses | Functions |
|---|---|---|---|---|
| SR-01 | E1 | Report an incident with description, media and GPS | P-2, P-3 | FR-01-01 |
| SR-02 | E1 | See the status of my report | P-4 | FR-01-02 |
| SR-03 | E1 | Report a missing person, pet or asset; help find them | P-5 | FR-01-03, FR-01-05 |
| SR-04 | E1 | Be alerted to relevant incidents near me | P-4 | FR-03-01 |
| SR-05 | O1 | Receive each report at the right agency without manual triage | P-1, P-2 | FR-02-02, FR-02-03 |
| SR-06 | O1 / HPC | Avoid duplicate reports | P-1 | FR-02-01 |
| SR-07 | O1 | Uncertain cases get verified before routing | P-2 | FR-02-04, FR-02-05 |
| SR-08 | Police | Be notified automatically about cases needing them; update case status | P-5 | FR-03-02, FR-01-04 |
| SR-09 | G2 / HPC | Oversee SLA performance and audit every state change | P-3 | FR-04-02, FR-04-03; FR-04-01 deferred |
| SR-10 | P1 / O3 | Connect through authenticated APIs and synced data | – | FR-05-01, FR-05-02, FR-05-03 |

## 6. Out of scope (see W0 Table 3)
Tax and other financial flows; the rest of the iHanoi platform; manual classification (replaced).

## 7. Open points
- Actor codes: the CVCA codes are the master. The use case diagram's ACT-G1/ACT-G2 are relabelled ACT-G2 (HPC) and ACT-O3 (LGSP) in these files; see the actor register in [w0-scope.md](w0-scope.md) §6.
- The CVCA has no stakeholder for the human reviewers who handle low-confidence reports (assumed HPC staff; open question #3).
