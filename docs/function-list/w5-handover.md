# W5 — Baseline and handover

> Lesson 5 · Source: `W5_Handover`, `W2_Rollup`, `Parameters`. Outputs: git tag `fl-v1.0`, the business case (sections 1–2), `tasks.md` seed (section 3, risk-first) and one consolidated open-questions table (section 4). The commit message should record that six of seven thresholds pass and the seventh (estimate variance) is REVIEW with the reason given in open question #6 ([w2-cost.md](w2-cost.md) §6).

## 1. Three scenarios

The three MoSCoW tiers **are** the three scenarios; no invented ±20%. The labels describe scope, not financial outcome: adding the Should tier costs 17.4M VND more and adds only the duplicate-avoidance benefit, so NPV falls ([w6-cash-flow.md](w6-cash-flow.md) §9).

| Scenario | Person-hours | Cost (VND) | Role in the sensitivity analysis |
|---|---|---|---|
| Pessimistic — Must only | 137.4 | 40,220,000 | Lowest investment and lowest benefit, yet the **highest NPV (46.7M) and shortest payback (2.1 years)** in [w6-cash-flow.md](w6-cash-flow.md) |
| Base — Must + Should | 196.8 | 57,640,000 | The client figure; base cash flow for NPV (39.2M, payback 2.7 years) |
| Optimistic — full scope | 237.4 | 69,020,000 | If the Could pool is never consumed; lowest NPV (17.8M, payback 3.5 years) |

## 2. Assumptions

| Assumption | Value | Source |
|---|---|---|
| Junior rate | 200,000 | ITviec 2025-26 salary report |
| Senior rate | 300,000 | ITviec 2025-26 salary report |
| Expert rate | 400,000 | ITviec 2025-26 salary report |
| AI leverage — FE / BE / INT | 0.35 / 0.7 / 1 | Class parameter, recalibrated each cohort |
| Verification factor — precise / vague | 0.15 / 0.4 | Class parameter |
| Hours per function point | 3 | Class parameter — used only for the cross-check |
| Team capacity (person-hours) | 240 | 4 people x 12 weeks x 5 h |
| Money contingency on the Must set | 0.1 | Covers estimation error only. Do NOT add 20% on top of the Could pool — same buffer twice. |

Money contingency is 10% on the Must tier only. It covers estimation error; do not add a further 20% on top of the Could pool (same buffer counted twice).

## 3. tasks.md seed: Must items, riskiest first

Risk order 1 = integration (unknowns touched early), 2 = back end, 3 = front end. 15 Must L3 items, 137.4 person-hours. Should and Could items follow in the same order once the Must set is green.

| L3 ID | Layer | Priority | Person-h | Risk order |
|---|---|---|---|---|
| FR-02-02-INT | INT | Must | 22.4 | 1 |
| FR-02-03-INT | INT | Must | 11.2 | 1 |
| FR-05-01-INT | INT | Must | 22.4 | 1 |
| FR-01-01-BE | BE | Must | 6.8 | 2 |
| FR-01-03-BE | BE | Must | 6.8 | 2 |
| FR-02-02-BE | BE | Must | 3.4 | 2 |
| FR-02-03-BE | BE | Must | 13.6 | 2 |
| FR-02-05-BE | BE | Must | 6.8 | 2 |
| FR-03-02-BE | BE | Must | 6.8 | 2 |
| FR-04-03-BE | BE | Must | 6.8 | 2 |
| FR-05-01-BE | BE | Must | 6.8 | 2 |
| FR-05-02-BE | BE | Must | 13.6 | 2 |
| FR-01-01-FE | FE | Must | 4 | 3 |
| FR-01-03-FE | FE | Must | 4 | 3 |
| FR-04-03-FE | FE | Must | 2 | 3 |

Risk order puts integrations first: the unknowns must be touched early, not at the end of DBIZ3.

## 4. Open questions

Paste each back into the owning feature's `spec.md`.

| # | [NEEDS CLARIFICATION] question | Blocking? | Owning feature | Default applied |
|---|---|---|---|---|
| 1 | [NEEDS CLARIFICATION: what is the source of the 0.9 confidence threshold (policy value or model default)?] | Yes | F-02 | 0.9 as written in FR-02-03 and FR-02-05 EARS |
| 2 | [NEEDS CLARIFICATION: is LGSP data read as a daily batch or in real time for cross-verification?] | Yes | F-02 | Daily batch sync |
| 3 | [NEEDS CLARIFICATION: who are the human reviewers (HPC staff roles), and do they need a reviewer screen? FR-02-05 has a BE row only.] | Yes | F-02 | No reviewer screen costed; team decision (see Team must confirm) |
| 4 | [NEEDS CLARIFICATION: should police be notified for every routed report, or only for missing-person / pet / asset cases? (FR-03-02 vs SQ-12)] | No | F-03 | Missing-person / pet / asset cases plus routed reports needing police attention (FR-03-02 EARS) |
| 5 | [NEEDS CLARIFICATION: what is the yearly report volume? 2,000 is a team coverage assumption (about 1.8% of the iHanoi flow) and drives NPV (CashFlow_NPV!B10).] | No | F-01 | 2,000 reports per year (team coverage assumption, Sources S19) |
| 6 *(added)* | [NEEDS CLARIFICATION: why do bottom-up (268 h) and function-point basis (443 h) differ by −39.5%, beyond the ±30% tolerance? Is the 3 h/FP parameter or an under-sized row the cause?] | No | all | Bottom-up retained as estimation basis (`Parameters` §9) |
| 7 *(added)* | [NEEDS CLARIFICATION: is running cost 20% of build a year (model, 11.5M VND) or the bottom-up 16.8M VND? Get a hosting quote.] | No | F-05 | 20% of build; bottom-up would cut base NPV to about 16.0M VND |

Three of the seven questions (#1–#3) are blocking and all three sit in F-02 (the AI routing feature, 38% of in-scope cost). Resolve them before the first F-02 sprint. Questions #6 and #7 were added in this write-up from the cost and cash-flow review; the workbook lists five.

The activity diagrams ([activity-diagrams/activity-index.md](activity-diagrams/activity-index.md) §5) list 12 blocking questions in total, 9 in F-02, 1 in F-01 (AD-UC-05) and 2 in F-05 (AD-UC-16). Most important new one: FR-02-05 (Must) depends on the LGSP result of FR-02-04 (Could); decide what handles low-confidence reports if FR-02-04 is not built.

## 5. Risks to carry into DBIZ3

- **Capacity:** the Ceiling (237.4 p-h) leaves 2.6 p-h (1.1%) of the 240 p-h capacity. Any Could item that grows, or any rejected Won't deferral, must be paid for by lowering another priority.
- **Estimate credibility:** the variance check is REVIEW (question #6).
- **Cash-flow inputs:** volume (2,000 reports a year), time saved per report and running cost are unverified; break-even volume is about 1,483 reports a year.
