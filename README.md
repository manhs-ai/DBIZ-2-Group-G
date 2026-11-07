# iHanoi Alert Module: DBIZ2 project documentation

Project documentation for **Group G**, DBIZ2 Digital Business Project II (VJCBI College, Foreign Trade University). It follows one product, the **iHanoi Alert Module**, from the stakeholder analysis through the cost workbook and the activity diagrams to the screens that DBIZ3 will build.

The Alert Module is one module inside the iHanoi city platform. Citizens report incidents (description, photo/video, GPS) or declare missing persons, pets and assets. The module checks for duplicates, classifies each report with AI and routes it to the responsible agency. Police and city agencies handle the cases, and HPC staff monitor SLA performance and the audit trail.

The core routing rule, used in many files: an AI **confidence score above 0.9** is auto-routed to the specialised agency under HPC. A score of **0.9 or below** is cross-checked with LGSP, flagged for human verification if LGSP does not confirm, and routed through Hanoi Portal. Exactly 0.9 takes the manual branch.

## Scope at a glance

| Item | Count / value |
| :---- | :---- |
| Use cases | 17 (UC-01 to UC-17) |
| Features | 5: F-01 Incident & Case Reporting, F-02 AI Classification & Smart Routing, F-03 Alerts & Notifications, F-04 Oversight & Analytics, F-05 Platform Integration & Access |
| Functions (FR-xx-yy) | 19, split into 31 work items (FE, BE, INT) |
| Actors | Citizens (ACT-E1), HPC & Government Agencies (ACT-G1), LGSP (ACT-G2), Technology Partners (ACT-P1), Police (ACT-P2) |
| Report statuses | Submitted, Routed, In Progress, Resolved |
| Currency | VND. Rates, thresholds and AI factors are the class Parameters of the workbook |

Feature owners (W0 Table 2): F-01 Owner A, F-02 Owner B, F-03 Owner C, F-04 Owner D, F-05 Owner E.

## Where each lesson's work goes

Each lesson writes into its own folder. This is the target layout; move the files you already have into it.

```
docs/
  README.md             this file
  context/              Lesson 3      CVCA v3, Context Diagram v3 (.drawio)
  use-case/             Lesson 4      Use Case Diagram v3, Sequence Diagram v3,
                                      UC-01 Sequence Diagram (.drawio)
  function-list/        Lessons 5-6   FL-Cost_Workshop_Group_G_final.xlsx
                                      (W0 to W5, Cash flow and NPV, Capital review)
  report/               Midterm       Midterm_Report_Final.docx
  activity/             Lesson 7      activity-index.md, AD-<UC-ID>.md
  external-design/      Lesson 8      screen-list.md, screen-transition.md
    screens/                          screen-spec-<Screen ID>.md
      img/                            <Screen ID>.png (numbered wireframes)
```

## Read it in this order

| Step | Lesson | File | What it shows |
| :---- | :---- | :---- | :---- |
| 1 | L3 | CVCA v3 | Stakeholders and flows BEFORE and AFTER the Alert Module (manual classification replaced by AI classification and auto-routing) |
| 2 | L3 | Context Diagram v3 | The module as one black box, five external entities, Flow-01 to Flow-10 |
| 3 | L4 | Use Case Diagram v3 | 17 use cases, five actors, the include and branch relations (UC-08 to UC-09 if above 0.9, to UC-13 if 0.9 or below) |
| 4 | L4 | UC-01 Sequence Diagram / Sequence Diagram v3 | Messages SQ-01 to SQ-19, with the duplicate and confidence alt branches. Each message names the FR it produces |
| 5 | L5 | W0_Scope sheet | Use case inventory, features, Won't-have list, data groups |
| 6 | L5 | W1_FunctionList sheet | Every function with its EARS criterion and work items |
| 7 | L5-L6 | W2 to W5, CashFlow_NPV, Capital_Review | Costing, cross-review, priorities, handover, cash flow and NPV |
| 8 | Midterm | Midterm_Report_Final.docx | The corrections applied to the diagrams and workbook, with the ID mapping |
| 9 | L7 | activity-index.md, then AD-UC-01 to AD-UC-17 | Inputs check against L3-L4, then one activity diagram per use case |
| 10 | L8 | screen-list.md | The eight screens (S-F01-01 to S-F01-06, S-F04-01, S-F04-02), the modals, and the use cases that have no screen |
| 11 | L8 | screen-transition.md | How each actor moves between screens, one diagram per actor |
| 12 | L8 | screens/screen-spec-S-F01-01 to -03 and img/ | Complete Screen Specs with numbered wireframes |

## How the files connect

Every file refers to the others by ID and never retells them:

- **UC-xx** use cases (W0 Table 1) are grouped into **F-xx** features (W0 Table 2).
- **FR-xx-yy** functions (W1) carry the EARS criterion. Work items add a suffix: FR-01-01-FE, -BE, -INT.
- **SQ-xx** sequence messages name the FR they produce.
- **AD-UC-xx** activity diagrams cite an FR on every system action. A call step to another use case cites the called use case's FR instead of copying its flow.
- **S-Fxx-nn** screens cite their use cases and FRs, and appear in the screen transitions.

### Activity diagrams (Lesson 7)

One `AD-UC-xx.md` per use case, written by the owner of its feature. Each file has the same sections: header table, diagram, node trace, branches and acceptance tests, object states, screens touched, open questions, verification log.

| Files | Feature | Lanes |
| :---- | :---- | :---- |
| AD-UC-01, 03, 05, 06, 11 | F-01 | Citizens & Community Users, Police or HPC where they act, iHanoi Alert Module |
| AD-UC-02, 07, 08, 09, 13, 17 | F-02 | iHanoi Alert Module, HPC & Government Agencies, LGSP |
| AD-UC-04, 10 | F-03 | iHanoi Alert Module, Citizens or Police |
| AD-UC-12, 14, 15 | F-04 | HPC & Government Agencies, iHanoi Alert Module |
| AD-UC-16 | F-05 | Technology Partners, iHanoi Alert Module |

Lane names come from the Context Diagram. There is one system lane, "iHanoi Alert Module", and no lane for a web app, API or database.

### External design (Lesson 8)

- Screen IDs follow `S-<feature>-<nn>`, for example S-F01-01 Report an Incident.
- There is no sign-in screen: sign-in belongs to the core iHanoi platform, which is outside the module.
- Some use cases deliberately have no screen: UC-04 (push notification only), UC-16 (API only), the system-only use cases, UC-12 (deferred). `screen-list.md` gives the reason for each.
- Full Screen Specs and wireframes exist for the Citizens' screens S-F01-01, S-F01-02 and S-F01-03. The other screens are named in the list and the transitions, and their specs belong to their owners.

## Notation

- Diagrams are Mermaid code blocks. The activity diagrams use `swimlane-beta`, which needs **Mermaid 11.16 or later**. If a diagram does not render in your viewer, paste the block into [mermaid.live](https://mermaid.live).
- The source diagrams (CVCA, Context, Use Case, Sequence) are `.drawio` files. Open them in diagrams.net.
- Final nodes may have several incoming flows. Every other node with two or more incoming flows goes through an explicit merge `{" "}`. Every fork has a matching join.
- `[NEEDS CLARIFICATION: ...]` marks a specification gap that no source file answers. It is not implemented behaviour. Where a default is applied, the open-questions table of that file states it.

## Known source-diagram gaps

These are recorded in the diagrams and in `activity-index.md`, so that nobody has to rediscover them:

- Context Diagram v3 has no entity for the AI Service, the push-notification service or Hanoi Portal, although the sequence diagram uses them. Hanoi Portal is treated as part of HPC & Government Agencies.
- The Use Case Diagram names the actor "Citizens", the Context Diagram "Citizens & Community Users". The lane uses the Context Diagram name.
- The sequence diagram calls the first report status "Received". W1 FR-01-02 says Submitted, and the activity diagrams follow W1.
- The sequence diagram covers UC-01 and the use cases it triggers. UC-03, UC-06, UC-14, UC-15, UC-16 and UC-17 have no sequence diagram of their own.

## Priorities and money

W1 holds the initial priorities and W4 the effective ones after the cost review. Where they differ, the AD header shows both (for example UC-03: Must in W1, Should in W4). Deferred items are recorded, not dropped: FR-03-03 (guidance and service-update notifications) and FR-04-01 (reports and analytics, UC-12).

The base NPV in the midterm report is 39,222,738 VND. It is conditional on the team's coverage assumption of 2,000 reports a year (1.8% of the iHanoi city-wide flow). Capital inputs are sourced in the `Sources` and `Capital_Review` sheets.
