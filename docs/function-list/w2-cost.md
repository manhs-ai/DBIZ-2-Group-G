# W2 — Cost

> Lesson 5 · Sources: `W2_Costing`, `W2_Rollup`, `Cost_per_Function`, `Parameters`. Fully derived: nothing is typed on these sheets. If a number looks wrong, fix the source (W1, W4 or Parameters) and recalculate.

## 1. Formula chain

```
person-hours = base hours × AI leverage + base hours × verification factor
cost (VND)   = person-hours × hourly rate of the seniority band
```

| Parameter | Value |
|---|---|
| Base hours S / M / L | 4 / 8 / 16 h |
| AI leverage FE / BE / INT | 0.35 / 0.7 / 1.0 (default band: Junior / Senior / Senior) |
| Rate Junior / Senior / Expert | 200,000 / 300,000 / 400,000 VND per hour |
| Verification factor (EARS precise / not precise) | 0.15 / 0.4 |
| Estimation basis | Bottom-up (switchable to function-point on `Parameters` §9) |

Worked example (`FR-02-02-INT`): L-size INT row = 16 base h; AI-adjusted 16 × 1.0 = 16 h; EARS not precise so verification 16 × 0.4 = 6.4 h; person-hours 22.4; Senior rate 300,000 → **6,720,000 VND**.

## 2. Cost per L3 work item

| L3 ID | Feature | L2 | Layer | Base h | AI factor | AI-adj h | Verif. factor | Verif. h | Person-h | Band | Rate (VND/h) | Cost (VND) | Effective priority |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| FR-01-01-FE | F-01 | FR-01-01 | FE | 8 | 0.35 | 2.8 | 0.15 | 1.2 | 4 | Junior | 200,000 | 800,000 | Must |
| FR-01-01-BE | F-01 | FR-01-01 | BE | 8 | 0.7 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300,000 | 2,040,000 | Must |
| FR-01-02-FE | F-01 | FR-01-02 | FE | 4 | 0.35 | 1.4 | 0.15 | 0.6 | 2 | Junior | 200,000 | 400,000 | Should |
| FR-01-02-BE | F-01 | FR-01-02 | BE | 4 | 0.7 | 2.8 | 0.15 | 0.6 | 3.4 | Senior | 300,000 | 1,020,000 | Should |
| FR-01-03-FE | F-01 | FR-01-03 | FE | 8 | 0.35 | 2.8 | 0.15 | 1.2 | 4 | Junior | 200,000 | 800,000 | Must |
| FR-01-03-BE | F-01 | FR-01-03 | BE | 8 | 0.7 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300,000 | 2,040,000 | Must |
| FR-01-04-FE | F-01 | FR-01-04 | FE | 4 | 0.35 | 1.4 | 0.15 | 0.6 | 2 | Junior | 200,000 | 400,000 | Should |
| FR-01-04-BE | F-01 | FR-01-04 | BE | 8 | 0.7 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300,000 | 2,040,000 | Should |
| FR-01-05-FE | F-01 | FR-01-05 | FE | 8 | 0.35 | 2.8 | 0.15 | 1.2 | 4 | Junior | 200,000 | 800,000 | Could |
| FR-01-05-BE | F-01 | FR-01-05 | BE | 4 | 0.7 | 2.8 | 0.15 | 0.6 | 3.4 | Senior | 300,000 | 1,020,000 | Could |
| FR-02-01-BE | F-02 | FR-02-01 | BE | 8 | 0.7 | 5.6 | 0.4 | 3.2 | 8.8 | Senior | 300,000 | 2,640,000 | Should |
| FR-02-02-BE | F-02 | FR-02-02 | BE | 4 | 0.7 | 2.8 | 0.15 | 0.6 | 3.4 | Senior | 300,000 | 1,020,000 | Must |
| FR-02-02-INT | F-02 | FR-02-02 | INT | 16 | 1 | 16 | 0.4 | 6.4 | 22.4 | Senior | 300,000 | 6,720,000 | Must |
| FR-02-03-BE | F-02 | FR-02-03 | BE | 16 | 0.7 | 11.2 | 0.15 | 2.4 | 13.6 | Senior | 300,000 | 4,080,000 | Must |
| FR-02-03-INT | F-02 | FR-02-03 | INT | 8 | 1 | 8 | 0.4 | 3.2 | 11.2 | Senior | 300,000 | 3,360,000 | Must |
| FR-02-04-INT | F-02 | FR-02-04 | INT | 16 | 1 | 16 | 0.4 | 6.4 | 22.4 | Senior | 300,000 | 6,720,000 | Could |
| FR-02-05-BE | F-02 | FR-02-05 | BE | 8 | 0.7 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300,000 | 2,040,000 | Must |
| FR-03-01-BE | F-03 | FR-03-01 | BE | 8 | 0.7 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300,000 | 2,040,000 | Should |
| FR-03-01-INT | F-03 | FR-03-01 | INT | 8 | 1 | 8 | 0.4 | 3.2 | 11.2 | Senior | 300,000 | 3,360,000 | Should |
| FR-03-02-BE | F-03 | FR-03-02 | BE | 8 | 0.7 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300,000 | 2,040,000 | Must |
| FR-03-03-BE | F-03 | FR-03-03 | BE | 4 | 0.7 | 2.8 | 0.4 | 1.6 | 4.4 | Senior | 300,000 | 1,320,000 | Won't |
| FR-04-01-FE | F-04 | FR-04-01 | FE | 8 | 0.35 | 2.8 | 0.4 | 3.2 | 6 | Junior | 200,000 | 1,200,000 | Won't |
| FR-04-01-BE | F-04 | FR-04-01 | BE | 8 | 0.7 | 5.6 | 0.4 | 3.2 | 8.8 | Senior | 300,000 | 2,640,000 | Won't |
| FR-04-02-FE | F-04 | FR-04-02 | FE | 8 | 0.35 | 2.8 | 0.15 | 1.2 | 4 | Junior | 200,000 | 800,000 | Could |
| FR-04-02-BE | F-04 | FR-04-02 | BE | 8 | 0.7 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300,000 | 2,040,000 | Could |
| FR-04-03-BE | F-04 | FR-04-03 | BE | 8 | 0.7 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300,000 | 2,040,000 | Must |
| FR-04-03-FE | F-04 | FR-04-03 | FE | 4 | 0.35 | 1.4 | 0.15 | 0.6 | 2 | Junior | 200,000 | 400,000 | Must |
| FR-05-01-INT | F-05 | FR-05-01 | INT | 16 | 1 | 16 | 0.4 | 6.4 | 22.4 | Senior | 300,000 | 6,720,000 | Must |
| FR-05-01-BE | F-05 | FR-05-01 | BE | 8 | 0.7 | 5.6 | 0.15 | 1.2 | 6.8 | Senior | 300,000 | 2,040,000 | Must |
| FR-05-02-BE | F-05 | FR-05-02 | BE | 16 | 0.7 | 11.2 | 0.15 | 2.4 | 13.6 | Senior | 300,000 | 4,080,000 | Must |
| FR-05-03-INT | F-05 | FR-05-03 | INT | 16 | 1 | 16 | 0.15 | 2.4 | 18.4 | Senior | 300,000 | 5,520,000 | Should |

**Totals (all 31 rows, Won't included):** base 268 h · AI-adjusted 193.4 h · verification 63.2 h · **256.6 person-hours · 74,180,000 VND**.

## 3. Cost per L2 function (client-facing)

| L2 ID | Function | Feature | FP type | L3 items | Person-h | Cost (VND) | Effective priority |
|---|---|---|---|---|---|---|---|
| FR-01-01 | Submit an incident report | F-01 | EI | 2 | 10.8 | 2,840,000 | Must |
| FR-01-02 | Track report status | F-01 | EQ | 2 | 5.4 | 1,420,000 | Should |
| FR-01-03 | Report a missing person, pet or asset | F-01 | EI | 2 | 10.8 | 2,840,000 | Must |
| FR-01-04 | Update case status (resolved / handed to police) | F-01 | EI | 2 | 8.8 | 2,440,000 | Should |
| FR-01-05 | Community sighting assistance | F-01 | EI | 2 | 7.4 | 1,820,000 | Could |
| FR-02-01 | Detect duplicate reports | F-02 | NC | 1 | 8.8 | 2,640,000 | Should |
| FR-02-02 | Classify report and score confidence | F-02 | EI | 2 | 25.8 | 7,740,000 | Must |
| FR-02-03 | Route report to the responsible agency | F-02 | EI | 2 | 24.8 | 7,440,000 | Must |
| FR-02-04 | Cross-verify report against LGSP data | F-02 | EQ | 1 | 22.4 | 6,720,000 | Could |
| FR-02-05 | Escalate to human verification on low confidence | F-02 | NC | 1 | 6.8 | 2,040,000 | Must |
| FR-03-01 | Send geofenced alert to nearby citizens | F-03 | EO | 2 | 18 | 5,400,000 | Should |
| FR-03-02 | Auto-notify the nearest police station | F-03 | EO | 1 | 6.8 | 2,040,000 | Must |
| FR-03-03 | Send guidance and service-update notifications | F-03 | EO | 1 | 4.4 | 1,320,000 | Won't |
| FR-04-01 | Generate reports and analytics for HPC & government agencies | F-04 | EO | 2 | 14.8 | 3,840,000 | Won't |
| FR-04-02 | Monitor SLA dashboard | F-04 | EQ | 2 | 10.8 | 2,840,000 | Could |
| FR-04-03 | Maintain and view audit trail | F-04 | NC | 2 | 8.8 | 2,440,000 | Must |
| FR-05-01 | Manage system integration & API access for technology partners | F-05 | NC | 2 | 29.2 | 8,760,000 | Must |
| FR-05-02 | Enforce role-based authorisation | F-05 | NC | 1 | 13.6 | 4,080,000 | Must |
| FR-05-03 | Provision environment and deploy the Alert Module | F-05 | NC | 1 | 18.4 | 5,520,000 | Should |

## 4. Roll-up per feature (after W4 priorities)

In-scope = Must + Should + Could (Won't excluded).

| Feature | Name | L2 | L3 | Base h | Must p-h | Should p-h | Could p-h | Won't p-h | In-scope p-h | In-scope cost (VND) | Must % | Transactional FP | Bottom-up h per FP |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| F-01 | Incident & Case Reporting | 5 | 10 | 64 | 21.6 | 14.2 | 7.4 | 0 | 43.2 | 11,360,000 | 50% | 20 | 3.2 |
| F-02 | AI Classification & Smart Routing | 5 | 7 | 76 | 57.4 | 8.8 | 22.4 | 0 | 88.6 | 26,580,000 | 65% | 12 | 6.3 |
| F-03 | Alerts & Notifications | 3 | 4 | 28 | 6.8 | 18 | 0 | 4.4 | 24.8 | 7,440,000 | 27% | 15 | 1.9 |
| F-04 | Oversight & Analytics | 3 | 6 | 44 | 8.8 | 0 | 10.8 | 14.8 | 19.6 | 5,280,000 | 45% | 13 | 3.4 |
| F-05 | Platform Integration & Access | 3 | 4 | 56 | 42.8 | 18.4 | 0 | 0 | 61.2 | 18,360,000 | 70% | 0 |  |

Project total: 19 L2, 31 L3, in-scope **237.4 person-hours, 69,020,000 VND**; Must share 57.9%.

## 5. The three headline figures

| Scenario | Scope | Person-hours | Cost (VND) | Meaning |
|---|---|---|---|---|
| Committed | Must only | 137.4 | 40,220,000 | What the team guarantees to deliver — the Minimum Usable SubseT |
| Expected | Must + Should | 196.8 | 57,640,000 | The figure given to the client; the base cash flow for Session 8 |
| Ceiling | Must + Should + Could | 237.4 | 69,020,000 | If the Could contingency pool is never consumed |

Always say which of the three you mean. Committed ≠ Expected ≠ Ceiling.

## 6. Threshold check

| Indicator | As written (W1) | After W4 | Threshold | Verdict |
|---|---|---|---|---|
| Must share of in-scope effort | 76.7% | 57.9% | ≤ 60% | **PASS** |
| Could share (contingency pool) | 4.6% | 17.1% | ≈ 20% | **PASS** |
| In-scope effort vs capacity (p-h) | 256.6 | 237.4 | ≤ 240 | **PASS** |
| Hidden technical work share | 29.2% | 29.2% | ≥ 10% | **PASS** |
| L3 rows above hour ceiling | 0 | 0 | 0 | **PASS** |
| Feature count | 5 | 5 | 3–6 | **PASS** |
| Bottom-up vs function-point variance (§7) | −39.5% | −39.5% | within ±30% | **REVIEW** |

Six of the seven checks pass; the variance check is REVIEW and is carried as open question #6 in [w5-handover.md](w5-handover.md). (The workbook's roll-up table lists only the first six.)

Before W4 the plan broke two limits (Must 76.7%, effort 256.6 p-h over the 240 capacity); the W4 priority changes fixed both without changing a single hour estimate. Headroom after W4 is thin: 2.6 person-hours (1.1%) of capacity.

## 7. Function-point cross-check

| Item | Value |
|---|---|
| Transactional FP (EI/EO/EQ, once per L2) | 60 |
| Data FP (ILF/EIF, [w0-scope.md](w0-scope.md) Table 4) | 61 |
| Unadjusted FP | 121 |
| Hours per FP (class parameter, pre-AI) | 3 |
| Functional work, FP estimate | 363 h |
| Non-functional (NC) work, bottom-up | 80 h |
| **FP-basis total** | **443 h** |
| **Bottom-up total (in use)** | **268 h** |
| Variance (bottom-up − FP) ÷ FP | **-39.5%** — workbook verdict: REVIEW (tolerance ±30%) |

Reading it: the two methods disagree by more than the tolerance, so the estimate should not be presented as settled. Two partial explanations are visible in the data. (1) The FP method counts all 7 data groups (61 FP, 183 h at 3 h/FP) once for the whole product, but bottom-up has no dedicated data-model L3 rows. (2) Per feature, bottom-up hours per FP range from 1.9 (F-03) to 6.3 (F-02); F-05 has 0 FP because every row is NC. The 3 h/FP figure is a class parameter and the workbook itself says to calibrate it. This write-up lists the check in §6 as the seventh threshold; the workbook shows it only on `W2_Costing`.

## 8. What the costing shows

- **Most expensive L3 rows:** FR-02-02-INT (22.4 p-h, 6,720,000 VND); FR-02-04-INT (22.4 p-h, 6,720,000 VND); FR-05-01-INT (22.4 p-h, 6,720,000 VND); FR-05-03-INT (18.4 p-h, 5,520,000 VND); FR-02-03-BE (13.6 p-h, 4,080,000 VND). The three 22.4 p-h rows are INT rows with vague EARS: they are expensive because they are *hard* (leverage 1.0) and *vague* (verification 0.4), not just large.
- **Cost of vague criteria:** the 9 not-precise L3 rows add about 23.0 verification hours (≈ 6,700,000 VND) compared with precise wording. Tightening EARS is the cheapest saving available.
- **Zero-FP work is expensive:** F-05 (all NC) is 18,360,000 VND in scope, 27% of in-scope cost, yet scores 0 function points.
- **AI leverage:** FE rows cost 35% of base hours in AI-adjusted time, INT rows 100%; the integration rows therefore dominate and are scheduled first (see [w5-handover.md](w5-handover.md)).
- **Check totals reconcile:** `Cost_per_Function` total 74,180,000 VND = `W2_Costing` total (check cell = 0). Won't cost excluded from scope: 5,160,000 VND.
