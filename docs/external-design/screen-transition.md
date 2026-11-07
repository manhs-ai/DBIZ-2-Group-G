# Screen Transition Diagram: iHanoi Alert Module

> DBIZ2 Lesson 8 · Nodes are Screen IDs from [screen-list.md](screen-list.md). One diagram per actor (the module has 13 screens, no actor sees more than 7). Sign in belongs to the core iHanoi platform, so every diagram starts after it. Technology Partners have no screen (API only, AD-UC-16).

## 1. Citizens

```mermaid
flowchart LR
  entry(["Entry: signed-in citizen opens the Alert Module in iHanoi"])
  push(["Entry: push notification of a nearby alert"])
  S00_01["S-F00-01<br>Alert home"]
  S01_01["S-F01-01<br>Submit incident report"]
  S01_02["S-F01-02<br>My reports"]
  S01_03["S-F01-03<br>Report status detail"]
  S01_04["S-F01-04<br>Report missing person / pet / asset"]
  S01_05["S-F01-05<br>Submit sighting"]
  S03_01["S-F03-01<br>Geofenced alert detail"]
  entry -->|"opens the Alert Module"| S00_01
  push -->|"taps the notification"| S03_01
  S00_01 -->|"taps Report an incident"| S01_01
  S01_01 -->|"[report sent] taps View my report"| S01_03
  S01_01 -->|"[report sent or duplicate notice] taps Back to home; or cancels"| S00_01
  S00_01 -->|"taps My reports"| S01_02
  S01_02 -->|"taps a report"| S01_03
  S01_03 -->|"taps back"| S01_02
  S01_02 -->|"taps back"| S00_01
  S00_01 -->|"taps Report missing person / pet / asset"| S01_04
  S01_04 -->|"[case created] taps Back to home; or cancels"| S00_01
  S00_01 -->|"taps an alert in the list"| S03_01
  S03_01 -->|"taps I saw them"| S01_05
  S01_05 -->|"[sighting sent] or taps back"| S03_01
  S03_01 -->|"taps back"| S00_01
```

## 2. Police

```mermaid
flowchart LR
  entry(["Entry: officer opens the Alert Module in iHanoi"])
  note(["Entry: in-app notification of a new case"])
  S00_01["S-F00-01<br>Alert home"]
  S03_02["S-F03-02<br>Police case notification"]
  S01_06["S-F01-06<br>Update case status"]
  entry -->|"opens the Alert Module"| S00_01
  note -->|"taps the notification"| S03_02
  S00_01 -->|"taps a case notification"| S03_02
  S03_02 -->|"taps Update case status"| S01_06
  S01_06 -->|"[status updated] or cancels"| S03_02
  S03_02 -->|"taps back"| S00_01
```

## 3. HPC & Government Agencies

```mermaid
flowchart LR
  entry(["Entry: HPC staff opens the Alert Module in iHanoi"])
  S00_01["S-F00-01<br>Alert home"]
  S02_01["S-F02-01<br>Verification queue"]
  S02_02["S-F02-02<br>Verify report category"]
  S04_01["S-F04-01<br>SLA dashboard"]
  S04_02["S-F04-02<br>Audit trail"]
  S01_06["S-F01-06<br>Update case status"]
  entry -->|"opens the Alert Module"| S00_01
  S00_01 -->|"taps Verification queue"| S02_01
  S02_01 -->|"taps a flagged report"| S02_02
  S02_02 -->|"[review sent] or taps back"| S02_01
  S00_01 -->|"taps SLA dashboard"| S04_01
  S04_01 -->|"taps a report row"| S04_02
  S00_01 -->|"taps Audit trail"| S04_02
  S04_02 -->|"taps Update case status"| S01_06
  S01_06 -->|"[status updated] or cancels"| S04_02
  S04_01 -->|"taps back"| S00_01
  S04_02 -->|"taps back"| S00_01
  S02_01 -->|"taps back"| S00_01
```

Conventions:

- Node ID: the Screen ID without the letter F and with `-` replaced by `_` (for example `S01_02` for `S-F01-02`, `S00_01` for `S-F00-01`); the label shows the real Screen ID on the first line.
- Arrow label: the user action ("taps Book"), or a system event in brackets ("[booking saved]").
- A modal is not a node. The modals "Location permission", "Discard report?" and "Confirm status change" are written in the Screen Specs, not here.
- Rounded stadium nodes are entry points from outside the module (a tile in iHanoi, a push or in-app notification).
- "Cancels" on a form returns to the screen it was opened from (through the modal "Discard report?" when the form has content).

## Checks before you save

- [x] Every screen in the Screen List appears in one or more of the diagrams (S-F00-01 in all three; S-F01-06 in Police and HPC).
- [x] Every screen is reachable from an entry point.
- [x] No dead end: every screen has a way out (back, home or a next step).
- [x] The main task of each actor reaches its result in three screens or fewer from the start screen: submit a report 1 (the result is a state of S-F01-01); follow a report 2; declare a missing case 1; handle a police notification 2; verify a report 2; check the SLA dashboard 1; view an audit trail 1.
- [x] Every arrow label is a user action or a system event, never a screen name.
- [x] Usage-flow paths: every arrow "UC-a to UC-b" in [usage-flow.md](usage-flow.md) has a path (table below).
- [ ] The diagram renders on mermaid.live. Syntax was parsed with Mermaid 12.3.0 in Node; mermaid.live was not run.

## Usage-flow paths

| Usage-flow arrow | Path on this diagram |
|---|---|
| UC-01 to UC-03 | S-F01-01 → S-F01-03 (taps View my report), or S-F00-01 → S-F01-02 → S-F01-03 |
| UC-04 to UC-11 | S-F03-01 → S-F01-05 (taps I saw them) |
| UC-10 to UC-06 | entry "in-app notification" → S-F03-02 → S-F01-06 (taps Update case status) |
| UC-14 to UC-15 | S-F04-01 → S-F04-02 (taps a report row) |
| UC-15 to UC-06 | S-F04-02 → S-F01-06 (taps Update case status) |
