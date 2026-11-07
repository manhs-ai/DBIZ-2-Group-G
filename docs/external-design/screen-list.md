# Screen List: iHanoi Alert Module

> DBIZ2 Lesson 8 · Sources: [usage-flow.md](usage-flow.md), the 17 activity diagrams ([activity-index.md](../activity-diagrams/activity-index.md)), W0 Table 1, W1 (FE rows), W4 priorities. Folder: `external-design/` (the template path is `docs/external-design/`).

Screen ID pattern: `S-<feature>-<nn>`, for example `S-F01-02` is the second screen of feature F-01. Screens shared by every feature use `S-F00-<nn>`. Sign in is provided by the core iHanoi platform and is not a screen of this module (the transition diagrams start after it).

13 screens: 1 shared, 6 in F-01, 2 in F-02, 2 in F-03, 2 in F-04 (none in F-05: partners use the API).

| Screen ID | Screen name | Actor | Feature | Shown when (purpose) | Entry points | Use cases / FRs served | Priority | Screen spec |
|---|---|---|---|---|---|---|---|---|
| S-F00-01 | Alert home | Citizens (ACT-E1); Police (ACT-P2); HPC & Government Agencies (ACT-G2) | F-00 | A signed-in user opens the Alert Module from iHanoi. | Alert Module tile on the iHanoi home screen (after sign in) | All use cases (hub); FR-05-02 | Must | `screens/screen-spec-S-F00-01.md` |
| S-F01-01 | Submit incident report | Citizens (ACT-E1) | F-01 | The citizen taps "Report an incident". | S-F00-01 | UC-01, UC-02; FR-01-01, FR-02-01 | Must | `screens/screen-spec-S-F01-01.md` |
| S-F01-02 | My reports | Citizens (ACT-E1) | F-01 | The citizen taps "My reports". | S-F00-01 | UC-03; FR-01-02 | Should | `screens/screen-spec-S-F01-02.md` |
| S-F01-03 | Report status detail | Citizens (ACT-E1) | F-01 | The citizen opens one of their reports. | S-F01-02; S-F01-01 (View my report) | UC-03; FR-01-02 | Should | `screens/screen-spec-S-F01-03.md` |
| S-F01-04 | Report missing person / pet / asset | Citizens (ACT-E1) | F-01 | The citizen taps "Report missing person / pet / asset". | S-F00-01 | UC-05; FR-01-03 (calls FR-03-02, FR-01-05) | Must | `screens/screen-spec-S-F01-04.md` |
| S-F01-05 | Submit sighting | Citizens (ACT-E1) | F-01 | A nearby citizen taps "I saw them" on an alert about an active missing case. | S-F03-01 (assumed) | UC-11; FR-01-05 | Could | `screens/screen-spec-S-F01-05.md` |
| S-F01-06 | Update case status | Police (ACT-P2); HPC & Government Agencies (ACT-G2) | F-01 | An authorised user chooses to update the status of a case. | S-F03-02; S-F04-02 | UC-06; FR-01-04 | Should | `screens/screen-spec-S-F01-06.md` |
| S-F02-01 | Verification queue | HPC & Government Agencies (ACT-G2), reviewer | F-02 | A reviewer taps "Verification queue". | S-F00-01 | UC-17; FR-02-05 | Must | `screens/screen-spec-S-F02-01.md` |
| S-F02-02 | Verify report category | HPC & Government Agencies (ACT-G2), reviewer | F-02 | A reviewer opens a flagged report. | S-F02-01 | UC-17; FR-02-05 (returns to FR-02-03) | Must | `screens/screen-spec-S-F02-02.md` |
| S-F03-01 | Geofenced alert detail | Citizens (ACT-E1) | F-03 | The citizen taps the push notification or an item in "RecentAlertsList". | Push notification; S-F00-01 (RecentAlertsList) | UC-04; FR-03-01 | Should | `screens/screen-spec-S-F03-01.md` |
| S-F03-02 | Police case notification | Police (ACT-P2) | F-03 | The police station is notified of a missing case or a routed report that requires police attention. | In-app notification (assumed channel); S-F00-01 (CaseNotificationsList) | UC-10; FR-03-02 | Must | `screens/screen-spec-S-F03-02.md` |
| S-F04-01 | SLA dashboard | HPC & Government Agencies (ACT-G2) | F-04 | An authorised HPC user taps "SLA dashboard". | S-F00-01 | UC-14; FR-04-02 | Could | `screens/screen-spec-S-F04-01.md` |
| S-F04-02 | Audit trail | HPC & Government Agencies (ACT-G2) | F-04 | An authorised HPC user opens the audit trail, or taps a row on the SLA dashboard. | S-F00-01; S-F04-01 | UC-15; FR-04-03 | Must | `screens/screen-spec-S-F04-02.md` |

## Modals and overlays

| Modal | Opened from screen | Purpose |
|---|---|---|
| Location permission | S-F01-01, S-F01-04, S-F01-05 | Asks the device for permission to use GPS (operating-system dialog) |
| Discard report? | S-F01-01, S-F01-04 | Confirms leaving a form that contains unsent content |
| Confirm status change | S-F01-06 | Confirms Resolved or Handed to Police before the update and its audit entry |

## Use cases without a screen

| Use case | Why no screen |
|---|---|
| UC-02 Duplicate Detection | System step; the citizen sees its result on S-F01-01 (duplicate notice) |
| UC-07 AI Classification, UC-08 Smart Routing, UC-09 Auto-route | System steps; no user interface |
| UC-13 LGSP Cross-Verification | System step plus a time event; no user interface |
| UC-16 Manage System Integration & API Access | Technology partners use the API, not a screen |
| UC-12 Generate Reports & Analytics | Won't (deferred, W0 Table 3): no screen is specified; add one only if the client confirms the feature |

## Screens without an FE work item in W1 (cost gap)

W1 has FE rows for FR-01-01, 01-02, 01-03, 01-04, 01-05, 04-01 (Won't), 04-02 and 04-03 only. Five screens found in Lesson 8 have no FE work item, so their effort is in no estimate. Nothing in W2 was changed; the table is indicative, assuming one S-size FE row each (4 base hours, 2.0 person-hours at the standard FE factors, Junior rate 200,000 VND per hour).

| Screen | Needed because | Priority | Indicative person-hours | Indicative cost (VND) |
|---|---|---|---|---|
| S-F00-01 Alert home | Every role needs a start screen; FR-05-02 shows only permitted actions | Must | 2.0 | 400,000 |
| S-F02-01 Verification queue | UC-17 reviewers need to find flagged reports (W5 question 3) | Must | 2.0 | 400,000 |
| S-F02-02 Verify report category | The reviewer's decision (AD-UC-17) | Must | 2.0 | 400,000 |
| S-F03-01 Geofenced alert detail | The citizen opens the alert that FR-03-01 sends | Should | 2.0 | 400,000 |
| S-F03-02 Police case notification | Police must see the notification FR-03-02 sends | Must | 2.0 | 400,000 |
| **Total** | | | **10.0** | **2,000,000** |

Effect if all five are added: in-scope effort 237.4 + 10.0 = 247.4 person-hours, 7.4 above the 240 capacity; Must share (137.4 + 8.0) ÷ 247.4 = 58.8%, still under 60%. Decide before DBIZ3: merge screens (for example one verification screen), demote S-F03-01 and S-F00-01 to a simpler form, or lower another priority. S-F01-02 and S-F01-03 share one 4-hour FE row (FR-01-02-FE "my reports list & status detail"), which is tight for two screens.

## Checks before you save

- [x] Every Screen ID is unique. No ID is used for two different screens.
- [x] No row is a state (error, empty, loading, success) or a confirmation modal. The duplicate notice, confirmation and "case created" panels are states of their screens.
- [x] Every Must use case has at least one screen, or a note saying it has no user interface (UC-07, 08, 09 and 16 above).
- [x] Every actor action in the activity diagrams maps to a screen in this list. The "Screens touched" tables of the AD files now carry these IDs. AD-UC-12 (Won't) and AD-UC-16 (API) have none.
- [x] Every screen serves at least one FR (S-F00-01 serves FR-05-02).
