# SD-UC-01 — Submit Incident Report

> Lesson 4 · Sources: `UC-01_Submit_Incident_Report_Sequence_Diagram.drawio` (basic flow) and `iHanoi_Alert_Sequence_Diagram_v3.drawio` (full flow, SQ-01…SQ-19). Message senders and receivers were read from arrow positions.

## A. Basic flow (steps 1–8, alt valid / invalid)

Participants: Citizen, iHanoi Mobile App, API Gateway, Alert Service (Backend), Database.

```mermaid
sequenceDiagram
  actor C as Citizen
  participant A as iHanoi Mobile App
  participant G as API Gateway
  participant S as Alert Service
  participant D as Database
  C->>A: 1. Submit incident report (description, photo/video, GPS)
  A->>G: 2. POST /alerts
  G->>S: 3. Authenticate & forward
  S->>S: 4. Validate report
  alt Report valid
    S->>D: 5. Save incident report (status = Submitted)
    D-->>S: 5.1 Confirmation
  else Invalid / incomplete
    S->>S: 5a. Reject report
  end
  S-->>G: 6. Return alertId & status / validation error
  G-->>A: 7. Return response
  A-->>C: 8. Show submission result
```

## B. Full flow (SQ-01…SQ-19)

Participants add AI Service (Classification), Notification Service and LGSP (external).

```mermaid
sequenceDiagram
  actor C as Citizen
  participant A as Mobile App
  participant G as API Gateway
  participant S as Alert Service
  participant AI as AI Service
  participant D as Database
  participant N as Notification Service
  participant L as LGSP
  C->>A: SQ-01 Submit report
  A->>G: SQ-02 Send issue data
  G->>S: SQ-03 Forward request
  S->>D: SQ-04 Duplicate check (location + time window)
  alt duplicate found
    S-->>G: SQ-05 Return duplicate notice (existing alertId, no new record)
    G-->>A: SQ-06 Show duplicate notice with existing alertId
  else no duplicate
    S->>D: SQ-07 Save report (status = Submitted)
    par confirmation to the citizen
      S-->>G: SQ-17 Return response (status, alertId)
      G-->>A: SQ-18 Relay response
      A-->>C: SQ-19 Show result & status
    and classification and routing
      S->>AI: SQ-08 Send for classification
      AI-->>S: SQ-09 Category + confidence
      S->>D: SQ-10 Save category & confidence
      alt confidence > 0.9
        S->>D: SQ-11 Auto-route to agency under HPC
        S->>N: SQ-12 Geofenced alert, police notice if needed
      else 0.9 or below
        S->>L: SQ-13 Cross-verify with LGSP data
        L-->>S: SQ-14 Cross-check result
        S->>D: SQ-15 Flag for human verification if not confirmed
        S->>D: SQ-16 Route via Hanoi Portal after review
      end
    end
  end
```

### Message trace

| Step | From → To | Message | Function |
|---|---|---|---|
| SQ-01 | Citizen → App | Submit report | FR-01-01-FE |
| SQ-02 | App → Gateway | Send issue data | FR-01-01-BE |
| SQ-03 | Gateway → Alert Service | Forward request | FR-05-01-INT (gateway); no own FR |
| SQ-04 | Alert Service → DB | Duplicate check | FR-02-01-BE (NC) |
| SQ-05 | Alert Service → Gateway | Duplicate notice | FR-02-01-BE |
| SQ-06 | Gateway → App | Show notice | FR-01-01-FE |
| SQ-07 | Alert Service → DB | Save report (status = Submitted) | FR-01-01-BE |
| SQ-08 | Alert Service → AI | Classify | FR-02-02-BE |
| SQ-09 | AI → Alert Service | Category + confidence | FR-02-02-INT |
| SQ-10 | Alert Service → DB | Save category | FR-02-02-BE |
| SQ-11 | Alert Service → DB | Auto-route | FR-02-03-BE |
| SQ-12 | Alert Service → Notification | Alert / police notice | FR-03-01-BE, FR-03-01-INT, FR-03-02-BE |
| SQ-13 | Alert Service → LGSP | Cross-verify | FR-02-04-INT |
| SQ-14 | LGSP → Alert Service | Result | FR-02-04-INT |
| SQ-15 | Alert Service → DB | Flag for human review | FR-02-05-BE |
| SQ-16 | Alert Service → DB | Route via Hanoi Portal | FR-02-03-BE, FR-02-03-INT |
| SQ-17 | Alert Service → Gateway | Return response (right after SQ-07, parallel with SQ-08 to SQ-16) | FR-01-01-BE |
| SQ-18 | Gateway → App | Relay | no own FR |
| SQ-19 | App → Citizen | Show result | FR-01-01-FE |

## Resolved conventions
1. **Initial status = Submitted** in both flows (the drawing of the full flow still says "Received").
2. **Duplicates:** the notice at SQ-05/06 ends the submission: no new record is created and the citizen sees the existing alertId (FR-02-01). The "confirm or cancel" choice drawn at SQ-06 is dropped; update the drawing.
3. **Hanoi Portal** receives manually routed reports (SQ-16); it is a lifeline-less external entity on the context diagram (Flow-11).

4. **Response timing:** the confirmation (SQ-17 to SQ-19) is sent right after SQ-07, in parallel with classification and routing, so the citizen does not wait for human review. Numbering is unchanged. Basis: FR-01-01, [AD-UC-01](activity-diagrams/AD-UC-01.md).

## Still open
1. **Invalid reports:** flow A rejects invalid reports; flow B has no validation step and no FR beyond "validate submission".
2. **Nothing returns to the app after routing:** the citizen sees status only by polling (FR-01-02); no step updates the app when routing, review or resolution completes.
3. **Human reviewer and Hanoi Portal** (SQ-15, SQ-16) are not lifelines; both end in a database write.
