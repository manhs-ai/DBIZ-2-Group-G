# W4 — Priorities

> Lesson 5 · Sources: `W3_CrossReview`, `W4_Prioritization`. Input: initial priorities from W1 and the defect log from W3. Output: one effective priority per L2 function, each change carrying the rule it followed and a written justification. The effective priority flows back into [w2-cost.md](w2-cost.md).

**The rule on the board: cut by lowering priority, never by lowering effort.** Effort changes only when the technical scope of a row genuinely changes, and what was dropped is written down. In this workbook no hour estimate was changed by W4.

## 1. Decision rules

| Rule | MoSCoW meaning | Test question | Usual Kano category |
|---|---|---|---|
| R1 · Must | The release has no viable use without it | If it is absent on delivery day, is the release unusable? | Must-be — invisible when present, fatal when absent |
| R2 · Should | Important, but a workaround exists | Painful to leave out, yet the solution still works? | One-dimensional — more of it, more satisfaction |
| R3 · Could | Desirable, low impact if dropped | Would anyone notice within the first month? | Attractive — a delighter, never a Must |
| R4 · Won't | Agreed as out of scope for this release | Did the client accept deferring it, and is it in W0 Table 3? | Indifferent — asked for, but nobody actually cares |

## 2. W3 cross-team review

Eight-criterion checklist, with the machine signals computed from the workbook (reviewer verdict cells are blank in the file):

| # | Criterion | Judged by | Machine signal |
|---|---|---|---|
| C1 | Two-way traceability | Machine + person | Features: 5  \|  see _coverage.md |
| C2 | Scope boundary explicit (Won't-have with a reason) | Person | Won't-have rows recorded: 5 |
| C3 | MECE — no duplicates, no missing CRUD | Person | Not machine-checkable — read it |
| C4 | State and time transitions have a trigger | Person | Read it — look for CLOCK-triggered functions |
| C5 | Hidden technical work | Machine + person | Hidden share: 29.2%  (floor 10%) |
| C6 | Granularity 4-16 hours | Machine | L3 rows above ceiling: 0 |
| C7 | Layer label FE / BE / INT present | Machine + person | L3 rows missing a layer: 0 |
| C8 | Independently testable EARS sentence | Person | Rows flagged EARS not precise: 9 |

Defect log (all three fixed; a review that says "looks fine" scores zero, minimum two evidenced defects):

| # | Location | Criterion | Evidence | Proposed bucket | Owning team response | Decision | Person-hours affected |
|---|---|---|---|---|---|---|---|
| 1 | FR-02-05 | C8 EARS | Threshold gap: FR-02-05 says "below 0.9" while FR-02-03 sends 0.9 or below to the manual path; exactly 0.9 had no branch in FR-02-05. | Fix now - align EARS text | Fixed (FR-02-05 EARS now "0.9 or below AND LGSP does not confirm") | Fixed | 6.8 |
| 2 | FR-04-03 | C3 MECE / classification | FR-04-03-FE "view audit trail" is a read-only inquiry, classified NC (0 FP); an inquiry is EQ. | Fix now - reclassify | Fixed (EQ, 4 FP) | Fixed | 2.0 |
| 3 | UC-06 / FR-01-04 | C3 MECE (actor) | W0 UC-06 primary actor = Citizens for "Update case status (resolved / handed to police)"; this is an authorised-staff action. | Fix now - correct actor | Fixed (Police; HPC & Government Agencies) | Fixed | 6.8 |

## 3. Effective priority per L2 function

Initial → effective: **9 of 19** functions changed. Counts before: Must 14, Could 2, Should 3. After: Must 9, Should 5, Could 3, Won't 2.

| Feature | L2 ID | Function | Initial | Rule applied | Kano | New priority | Effective | Justification | Person-h | Cost (VND) |
|---|---|---|---|---|---|---|---|---|---|---|
| F-01 | FR-01-01 | Submit an incident report | Must | R1 Must | Must-be |  | Must | Unchanged - without incident submission the release has no viable use (R1). | 10.8 | 2,840,000 |
| F-01 | FR-01-02 | Track report status | Must | R2 Should | One-dimensional | Should | Should | Demoted Must to Should (R2): submission already returns a confirmation with alert ID and status (FR-01-01), so a workaround exists. Also closes W3 defect #1 (overlap with FR-01-01): kept as a separate function and cross-referenced to FR-01-01. Effort unchanged. | 5.4 | 1,420,000 |
| F-01 | FR-01-03 | Report a missing person, pet or asset | Must | R1 Must | Must-be |  | Must | Unchanged - the missing person/pet/asset case (UC-05) is a core use case and drives police notification (R1). | 10.8 | 2,840,000 |
| F-01 | FR-01-04 | Update case status (resolved / handed to police) | Must | R2 Should | One-dimensional | Should | Should | Demoted Must to Should (R2): agencies can update case status in their existing process until this ships; the audit entry itself is covered by FR-04-03, which stays Must. Effort unchanged. | 8.8 | 2,440,000 |
| F-01 | FR-01-05 | Community sighting assistance | Could | R3 Could | Attractive |  | Could | Unchanged - nobody would notice within the first month, and it needs active missing-person cases first (R3). | 7.4 | 1,820,000 |
| F-02 | FR-02-01 | Detect duplicate reports | Must | R2 Should | One-dimensional | Should | Should | Demoted Must to Should (R2): the receiving agency can merge duplicates manually and the solution still works. EARS is not precise - clarify the location radius and time window before build. Effort unchanged. | 8.8 | 2,640,000 |
| F-02 | FR-02-02 | Classify report and score confidence | Must | R1 Must | Must-be |  | Must | Unchanged - AI classification and the confidence score drive every routing decision (R1). | 25.8 | 7,740,000 |
| F-02 | FR-02-03 | Route report to the responsible agency | Must | R1 Must | Must-be |  | Must | Unchanged - routing to the responsible agency is the purpose of the module (R1). Clarify the exact 0.9 boundary (this row: 0.9 or below = manual; FR-02-05: below 0.9 = human verification). | 24.8 | 7,440,000 |
| F-02 | FR-02-04 | Cross-verify report against LGSP data | Should | R3 Could | Attractive | Could | Could | Demoted Should to Could (R3): routing works without LGSP cross-verification, and the RAG lookup is the most expensive Should (22.4 p-h). Moving it restores the Could contingency pool above its floor. Effort unchanged. | 22.4 | 6,720,000 |
| F-02 | FR-02-05 | Escalate to human verification on low confidence | Must | R1 Must | Must-be |  | Must | Unchanged - low-confidence reports need a human path before routing; without it the 0.9-or-below branch has no handler (R1). | 6.8 | 2,040,000 |
| F-03 | FR-03-01 | Send geofenced alert to nearby citizens | Must | R2 Should | One-dimensional | Should | Should | Demoted Must to Should (R2): reports are still routed to agencies and police without public alerts; geofenced alerts add value but are not needed for first use. Effort unchanged (18 p-h). | 18 | 5,400,000 |
| F-03 | FR-03-02 | Auto-notify the nearest police station | Must | R1 Must | Must-be |  | Must | Unchanged - nearest-station notification is how missing-person cases reach police (R1). | 6.8 | 2,040,000 |
| F-03 | FR-03-03 | Send guidance and service-update notifications | Could | R4 Won't | Indifferent | Won't | Won't | Demoted Could to Won't (R4): lowest value, trigger undefined (EARS not precise) and only one source (CTX:flow-04). Recorded in W0 Table 3, pending client confirmation. Needed to bring in-scope effort within team capacity. | 4.4 | 1,320,000 |
| F-04 | FR-04-01 | Generate reports and analytics for HPC & government agencies | Should | R4 Won't | Indifferent | Won't | Won't | Demoted Should to Won't (R4): the SLA dashboard and audit trail already give HPC oversight of routing and resolution, so report generation is deferred. Recorded in W0 Table 3, pending client confirmation. Needed to bring in-scope effort within team capacity. | 14.8 | 3,840,000 |
| F-04 | FR-04-02 | Monitor SLA dashboard | Should | R3 Could | Attractive | Could | Could | Demoted Should to Could (R3): HPC can follow SLA through the audit trail until the dashboard ships. Keeps the Could pool at or above its floor. Effort unchanged. | 10.8 | 2,840,000 |
| F-04 | FR-04-03 | Maintain and view audit trail | Must | R1 Must | Must-be |  | Must | Unchanged - every state change must be auditable; compliance requirement, release not acceptable to HPC without it (R1). | 8.8 | 2,440,000 |
| F-05 | FR-05-01 | Manage system integration & API access for technology partners | Must | R1 Must | Must-be |  | Must | Unchanged - authenticated API access is how technology partners and external services connect to the module (R1). | 29.2 | 8,760,000 |
| F-05 | FR-05-02 | Enforce role-based authorisation | Must | R1 Must | Must-be |  | Must | Unchanged - role-based access protects citizen data for every actor; unsafe to release without it (R1). | 13.6 | 4,080,000 |
| F-05 | FR-05-03 | Provision environment and deploy the Alert Module | Must | R2 Should | One-dimensional | Should | Should | Demoted Must to Should (R2): staging can be deployed manually for the first release; automated CI/CD is valuable hidden work but a workaround exists. Effort unchanged (18.4 p-h). | 18.4 | 5,520,000 |

## 4. Control panel after the decisions

| Indicator | Current | Threshold | Verdict |
|---|---|---|---|
| Must share of in-scope effort | 57.9% | 60% | PASS |
| Could share (contingency pool) | 17.1% | 20% | PASS |
| In-scope effort vs capacity (p-h) | 237.4 | 240 | PASS |

The bottom-up vs function-point variance check (−39.5%, REVIEW) is not on this panel; see [w2-cost.md](w2-cost.md) §6 and open question #6 in [w5-handover.md](w5-handover.md).

Expected cost (Must + Should) = **57,640,000 VND**, the figure given to the client and used for NPV in [w6-cash-flow.md](w6-cash-flow.md).

## 5. Pending client confirmation

- FR-03-03 (guidance and service-update notifications) → Won't. Trigger undefined, one source only.
- FR-04-01 (reports and analytics) → Won't. SLA dashboard and audit trail already give HPC oversight.

Both are recorded in [w0-scope.md](w0-scope.md) Table 3. If the client rejects either deferral, the Could pool (17.1% of scope effort) and the 2.6 h capacity headroom cannot absorb them; something else must be demoted.
