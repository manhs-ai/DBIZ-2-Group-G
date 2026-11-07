# AI log: where AI suggested something wrong

iHanoi Alert Module, Group G, DBIZ2 Digital Business Project II.

Kept for the whole project. One entry each time an AI tool proposed something the team did not accept as it was, with the decision and where it was recorded. Entries 1 to 9 cover the AD-UC-01 to AD-UC-03 files, AD-UC-16 and AD-UC-17 (revised copies), the Lesson 8 screens and this documentation. Each owner adds the entries for their own files.

| # | Lesson | What AI suggested | Why it was wrong for iHanoi Alert Module | What the team did |
| :---- | :---- | :---- | :---- | :---- |
| 1 | L7 (AD-UC-01) | First render placed the end node and the "Receive confirmation" box outside the Citizens lane | A step in the wrong lane says the wrong actor does it. The final node belongs to the system lane, and confirmation is an actor step inside the Citizens lane | End node moved into the system lane, diagram re-rendered. Lane check recorded in the verification log of AD-UC-01 |
| 2 | L7 (AD-UC-01) | The duplicate notice (s4) cited FR-01-01 only | The notice is produced by the duplicate check, FR-02-01 (SQ-05). A system action must cite the FR that produces it. A separate reviewer found it, not the author | s4 now cites FR-01-01 and FR-02-01. The Critic finding is logged in AD-UC-01 section 6 |
| 3 | L7 (AD-UC-01, AD-UC-03) | Actor steps named "Review duplicate notice" and "Read current status" | They read like screen actions (a UI step), but an activity diagram describes behaviour. The detail belongs to Lesson 8 | Renamed "Receive duplicate notice" and "Receive current status". Screens are listed in section 5 and in the screen list |
| 4 | L7 (AD-UC-03) | The Critic row of AD-UC-03 repeated the findings of AD-UC-01 | The log would claim findings that do not belong to AD-UC-03, so it could not be trusted as evidence | Row rewritten with AD-UC-03's own single finding. Each log is now checked against its own diagram |
| 5 | L7 (AD-UC-01) | The verification log gave the node count as 22, then 24. A duplicated intro line and a stray sentence about "client" items stayed in section 6 | The diagram has 23 nodes. Numbers in the log must match the diagram, and leftover text from a draft is not a record | Count corrected to 23, duplicate line and stray sentence removed, file re-sent |
| 6 | L7 (AD-UC-02) | Chose 100 m, 24 hours and an inclusive boundary for "same location within a time window" | FR-02-01 gives no values. W4 asks the client to fix them before build. AI-chosen numbers must not read as client requirements | Kept only as configurable defaults, marked "team assumption to state to the client" in AD-UC-02 and in the branch tests. Still to be confirmed by the client |
| 7 | L7 (AD-UC-16, AD-UC-17) | Draft logs said the Context Diagram was not supplied and left 9 and 10 open questions | The Context Diagram v3 is in the project, with the lane names "Technology Partners" and "HPC & Government Agencies" | Lane names checked against the Context Diagram. Open questions closed with defaults from W1, W4 and the midterm mapping (Hanoi Portal sits inside HPC & Government Agencies) |
| 8 | L8 (wireframes) | Numbered badges were clipped at the edge or covered the element labels | The badge numbers must match the element table of the Screen Spec, so a covered label breaks the link | Badges moved to the right edge and offset from the chips. S-F01-01 to S-F01-03 images regenerated |
| 9 | L8 (README) | The README listed AD-UC-02 under both F-01 and F-02 | W0 Table 2 puts UC-02 in F-02 only | Removed from the F-01 row |


