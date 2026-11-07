# Project brief — iHanoi Alert Module

> Lessons 1–2. Reconstructed from the uploaded diagrams and the cost workbook; no original brief document was provided. Items marked **[assumed]** or **[to confirm]** are not stated in the sources.

## Client
- **Hanoi People's Committee (HPC)** and the specialised departments and agencies under it, including Police, operating the **iHanoi** citizen application. **[assumed]** HPC expands to Hanoi People's Committee; the sources only write "HPC".
- Related parties: Hanoi People's Council (authorises HPC), Hanoi Portal, LGSP (government data and integration platform), technology partners (e.g. Viettel), Hanoi citizens. See [cvca.md](cvca.md).

## Problem
Today a citizen's complaint goes through iHanoi and is **classified manually**. The CVCA "before" state records the symptoms: *unclear route*, *slow response*, *passive response*. Citizens get little feedback, and missing-person, pet and asset cases have no dedicated flow.

## Goal
Build the **iHanoi Alert Module**, a crowdsourcing alert module integrated into iHanoi (not a rebuild of iHanoi or Hanoi Portal) that:
1. lets citizens submit incident reports (description, photo/video, GPS) and missing person/pet/asset cases, and track their status;
2. detects duplicates, classifies each report with AI and a confidence score, and routes it to the responsible agency: automatically above 0.9, otherwise via LGSP cross-check and human verification;
3. sends geofenced alerts to nearby citizens and notifies the nearest police station;
4. gives HPC an audit trail and SLA dashboard, and gives technology partners authenticated API access.

Scope boundaries and deferrals are in [w0-scope.md](w0-scope.md).

## Constraints and targets (from the workbook)
| Item | Value |
|---|---|
| Build team capacity (DBIZ3) | 4 people × 12 weeks × 5 productive h = **240 person-hours** |
| Committed scope (Must) | 137.4 p-h · 40,220,000 VND |
| Expected scope (Must + Should) | 196.8 p-h · 57,640,000 VND |
| Ceiling (+ Could) | 237.4 p-h · 69,020,000 VND |
| Base business case | NPV 39.2 M VND, IRR 24.9%, payback 2.7 years (see [w6-cash-flow.md](w6-cash-flow.md)); Must-only has a higher NPV (46.7 M) and shorter payback (2.1 years) |

## Team and roles
| Member | Role |
|---|---|
| Lê Minh Anh | Owner A: F-01 Incident & Case Reporting |
| Nguyễn Tương Như | Owner B: F-02 AI Classification & Smart Routing |
| Trần Khánh Ngọc | Owner C: F-03 Alerts & Notifications |
| Phạm Khánh Linh | Owner D: F-04 Oversight & Analytics |
| Vân Thuỷ | Owner E: F-05 Platform Integration & Access |
| Vũ Thu Phương | Cost estimation, cash flow, NPV, IRR, payback, break-even, financial cross-check, contribution consolidation |

Owners are responsible for their feature's functions, costs and priorities. (The workbook lists six members; the 240 h capacity assumes four builders, so confirm who builds.)

## Milestones
No calendar dates are given for the project; the sequence below follows the course lessons and the workbook.

| Step | Lesson | Output |
|---|---|---|
| 1 | L1–L2 | This brief |
| 2 | L3 | CVCA, context diagram |
| 3 | L4 | Use case diagram, sequence diagrams, architecture and ADRs |
| 4 | L5 (Session 7) | W0 scope → W1 function list → W2 cost → W3 cross-review → W4 priorities → W5 handover (tag `fl-v1.0`) |
| 5 | L6 (Session 8) | Cash flow, NPV, IRR, payback for three scenarios |
| 6 | DBIZ3 | 12-week build, Must items first, integrations first |

External sources in the workbook were accessed on 2026-10-10 and 2026-10-11.

## Open questions
See [w5-handover.md](w5-handover.md) §4 (seven questions; #6 and #7 were added in this write-up). The three blocking ones concern the 0.9 threshold source, the LGSP read mode, and the human-reviewer role.

## Key risks
- **Capacity headroom is 2.6 person-hours** at the Ceiling (237.4 of 240).
- **Estimate variance:** bottom-up and function-point estimates differ by −39.5% (tolerance ±30%).
- **Cash-flow inputs are unverified** (volume, time saved, running cost); base NPV drops to about 16.0M VND with the bottom-up running cost, and break-even volume is about 1,483 reports a year.
- **Blocking questions:** the three in W5 are in F-02 (38% of in-scope cost). The activity diagrams add more: 12 in total, 9 in F-02, 1 in F-01 and 2 in F-05 ([activity-index.md](activity-diagrams/activity-index.md) §5).
