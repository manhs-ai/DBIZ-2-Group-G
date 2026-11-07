# W6 — Cash flow, NPV, IRR, payback

> Lesson 6 · Sources: `CashFlow_NPV`, `Capital_Review`, `Sources`. Period = year, currency = VND. Year 0 = build cost (the W5 scenario) + a 10% money contingency on the Must tier as its own line. Years 1–5 = monetised benefits − operating cost, flat (no growth or ramp-up: no source for either). Base case = Must + Should = 57,640,000 VND.

## 1. Inputs

| Input | Value | Unit | Basis |
|---|---|---|---|
| Horizon (years after go-live) | 5 | years | ASSUMPTION (team) |
| Discount rate (base) | 0.04362 | % p.a. | SOURCED |
| Sensitivity step (±) | 0.02 | percentage points | ASSUMPTION (spec) |
| Money contingency on Must tier | 0.1 | % of Must cost | TEAM DECISION |
| Operating cost per year (maintenance & hosting) | 0.2 | % of build / yr | SOURCED (central of a 15–25% range) |
| Reports per year (volume) | 2000 | reports / yr | TEAM ASSUMPTION — UNVERIFIED |
| Routing time saved per report (agency staff) | 0.25 | hours / report | ASSUMPTION — UNVERIFIED |
| Share of reports that are duplicates | 0.1 | % | ASSUMPTION — UNVERIFIED |
| Handling time per duplicate avoided | 0.5 | hours / duplicate | ASSUMPTION — UNVERIFIED |
| Average monthly income of workers (national, 2025) | 8310000 | VND / month | SOURCED (proxy) |
| Loaded value of one hour of staff time | 57,367 | VND / hour | DERIVED |
| Scenario selector (1 = Must, 2 = Must + Should [BASE], 3 = Full scope) | 2 | 1 / 2 / 3 | INPUT |
| Selected scenario | Must + Should (BASE) |  |  |
| Employer on-cost on wage (social insurance, BHYT, BHTN, work injury) | 0.215 | % of wage | SOURCED (secondary) |

## 2. Scenario build

| Line | Must only | Must + Should (BASE) | Full scope |
|---|---|---|---|
| Build cost, W5 scenario (VND) | 40220000 | 57640000 | 69020000 |
| Must-tier cost (contingency base, VND) | 40220000 | 40220000 | 40220000 |
| Money contingency line = Must-tier cost × B8 (VND) | 4022000 | 4022000 | 4022000 |
| YEAR 0 outflow = −(build + contingency) (VND) | -44242000 | -61662000 | -73042000 |
| Effective priority of FR-02-03 (routing) | Must | Must | Must |
| Effective priority of FR-02-01 (duplicate detection) | Should | Should | Should |
| Routing benefit included? (1/0) | 1 | 1 | 1 |
| Duplicate-avoidance benefit included? (1/0) | 0 | 1 | 1 |
| Benefit 1 — routing time saved (VND / yr) | 28,683,665 | 28,683,665 | 28,683,665 |
| Benefit 2 — duplicate handling avoided (VND / yr) | 0 | 5,736,733 | 5,736,733 |
| Total monetised benefit (VND / yr) | 28,683,665 | 34,420,398 | 34,420,398 |
| Operating cost (VND / yr) | -8044000 | -11528000 | -13804000 |
| Net annual cash flow, years 1–5 (VND / yr) | 20,639,665 | 22,892,398 | 20,616,398 |

Benefit formulas: routing = volume × hours saved × loaded hour value; duplicates = volume × duplicate share × handling hours × loaded hour value. Loaded hour value = 57,367 VND = 8,310,000 × 1.215 ÷ 176.

## 3. Timeline (VND)

| Year | t | Must CF | Must+Should CF | Full CF | Cum. Must | Cum. Must+Should | Cum. Full |
|---|---|---|---|---|---|---|---|
| Year 0 | 0 | -44,242,000 | -61,662,000 | -73,042,000 | -44,242,000 | -61,662,000 | -73,042,000 |
| Year 1 | 1 | 20,639,665 | 22,892,398 | 20,616,398 | -23,602,335 | -38,769,602 | -52,425,602 |
| Year 2 | 2 | 20,639,665 | 22,892,398 | 20,616,398 | -2,962,670 | -15,877,205 | -31,809,205 |
| Year 3 | 3 | 20,639,665 | 22,892,398 | 20,616,398 | 17,676,994 | 7,015,193 | -11,192,807 |
| Year 4 | 4 | 20,639,665 | 22,892,398 | 20,616,398 | 38,316,659 | 29,907,591 | 9,423,591 |
| Year 5 | 5 | 20,639,665 | 22,892,398 | 20,616,398 | 58,956,324 | 52,799,989 | 30,039,989 |

## 4. Results

| Metric | Must only | Must + Should (BASE) | Full scope |
|---|---|---|---|
| NPV at base rate (VND)  = CF0 + NPV(rate, CF1:CF5) | 46,715,146 | 39,222,738 | 17,812,611 |
| IRR | 0.3698 | 0.2492 | 0.127 |
| Cumulative cash flow at year 5 (VND) | 58,956,324 | 52,799,989 | 30,039,989 |
| Last year with negative cumulative cash flow (k; −1 = none) | 2 | 2 | 3 |
| Payback (years) = k + \|cum(k)\| ÷ CF(k+1) | 2.144 | 2.694 | 3.543 |
| Sensitivity: NPV at rate − 2 pp (VND) | 52,028,454 | 45,115,971 | 23,119,929 |
| Sensitivity: NPV at rate + 2 pp (VND) | 41,850,227 | 33,826,833 | 12,953,175 |

Formulas: NPV = CF0 + NPV(rate, CF1:CF5) at 4.362% (10-year government benchmark yield, VBMA June 2026); payback = k + |cum(k)| ÷ CF(k+1), where k is the last year with negative cumulative cash flow. Target test (base): NPV > 0 **YES**, payback ≤ 5 years **YES**.

## 5. Break-even

| Line | Must only | Must + Should (BASE) | Full scope |
|---|---|---|---|
| Required net annual cash flow for NPV = 0 (VND / yr) | 10,039,234 | 13,992,117 | 16,574,425 |
| Required gross benefit per year (VND / yr) = net + operating cost | 18,083,234 | 25,520,117 | 30,378,425 |
| Monetised value per report (VND) | 14,342 | 17,210 | 17,210 |
| Breakeven volume (reports / yr) for NPV = 0 | 1,261 | 1,483 | 1,765 |
| Placeholder volume in B10 (reports / yr) | 2000 | 2000 | 2000 |

Base break-even volume is 1,483 reports per year, 74% of the assumed 2,000, i.e. 1.34% of the iHanoi city-wide flow (about 110,000 a year, annualised from 94,000+ reports since 1 July 2025).

## 6. Sensitivity of base NPV

| Case | Base NPV (VND) |
|---|---|
| Discount rate − 2 pp | 45,115,971 |
| Discount rate + 2 pp | 33,826,833 |
| Monetised benefit − 20% | 8,885,218 |
| Monetised benefit + 20% | 69,560,257 |
| Running cost at bottom-up 16,788,000 VND a year *(this write-up's calculation)* | 16,042,391 |

Leverage of each input, ranked by NPV change for a +10% relative move:

| Rank | Lever | Cell | Current | Status | ΔNPV for +10% (VND) |
|---|---|---|---|---|---|
| 1 | Reports per year | B10 | 2000 | TEAM ASSUMPTION — UNVERIFIED | 15,168,760 |
| 2 | Value of one staff hour (income and on-cost) | B14, B18, B15 | 57,367 | SOURCED (proxy) | 15,168,760 |
| 3 | Routing time saved per report | B11 | 0.25 | ASSUMPTION — UNVERIFIED | 12,640,633 |
| 4 | Running cost (share of build per year) | B9 | 0.2 | SOURCED (central of a 15–25% range) | -5,080,286 |
| 5 | Employer on-cost uplift | B18 | 0.215 | SOURCED (secondary) | 2,684,184 |
| 6 | Duplicate share | B12 | 0.1 | ASSUMPTION — UNVERIFIED | 2,528,127 |
| 6 | Handling time per duplicate | B13 | 0.5 | ASSUMPTION — UNVERIFIED | 2,528,127 |
| 8 | Discount rate | B6 | 0.04362 | SOURCED | -1,231,081 |

## 7. Benefits not monetised

- Faster police notification for missing-person/pet/asset cases (FR-03-02): no defensible unit value.
- Geofenced alerts (FR-03-01, Should): awareness effect real but unmeasured.
- LGSP cross-verification (FR-02-04, Could): accuracy gain; no rework-cost source.
- Audit trail (FR-04-03, Must): compliance value, not a cash benefit.

## 8. Sanity checks (base)

- **Freed capacity:** 600 staff hours a year = 0.28 FTE. It becomes cash only if posts are cut or staff are redeployed; the workbook calls this an economic value, not a cash saving. Confirm the reading with the lecturer.
- **Volume:** 2,000 reports a year is a team assumption (S19, unverified), 1.8% of the city-wide flow. It is the biggest lever.
- **Running cost:** model 11,528,000 VND a year (20% of build) vs a bottom-up minimum of 16,788,000 VND (one small VM at 599,000 VND a month + 4 h a month support at the Junior rate): the model is 69% of the bottom-up figure.
- **Per report:** 17,210 VND = 0.3 staff hours a report.

## 9. Reading the results (added by this write-up, not in the workbook)

- **Scenario names are about scope, not outcome.** "Pessimistic" (Must only) has the *highest* NPV (46,715,146) and IRR (37.0%), and the shortest payback (2.14 years). Adding Should costs 17,420,000 VND more but adds only the duplicate-avoidance benefit (5,736,733 VND a year); the other Should items (status tracking, case-status update, geofenced alerts, CI/CD) carry no monetised benefit. Base NPV is 7,492,408 VND lower than Must-only. The case for the Should tier rests on the unmonetised benefits in section 7.
- **Running-cost risk.** Replacing the 20% model with the bottom-up 16,788,000 VND would cut the base net cash flow to 17,632,398 VND a year, base NPV to about **16,042,391 VND** (my calculation, still positive), and base payback to roughly 3.5 years.
- **Benefit risk is larger than rate risk.** A 20% lower benefit takes base NPV from 39,222,738 to 8,885,218 VND; a ±2 pp discount rate moves it by under 6 million.
- **What would settle it:** a measured count of alert-type reports in iHanoi or hotline data, a measured triage time per report, and a hosting quote.

## 10. Source status

Verified: ITviec salary bands, IFPUG weights (S07–S11), DSDM thresholds, VBMA yield, GSO income (secondary), social-insurance rate (secondary), maintenance 15–25% (practitioner source). **Unverified:** overhead factor 1.35 (S04), 2,000 reports a year, 0.25 h saved, duplicate share and handling time (S19), 4 h a month support (S31). Full list with URLs and access dates (2026-10-10/11) is on the `Sources` sheet.
