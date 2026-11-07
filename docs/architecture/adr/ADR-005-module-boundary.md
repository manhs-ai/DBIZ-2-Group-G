# ADR-005 — Alert Module is a bounded module inside iHanoi

- **Status:** Proposed (Context Diagram boundary; W0 Won't-have)
- **Context:** The context diagram's box is "iHanoi Alert Module", not the iHanoi platform; the CVCA shows tax and budget flows at governance level.
- **Decision:** The module reads citizen identity, agency directory and LGSP data (EIFs) and maintains only its own data (4 ILFs). It does not rebuild Hanoi Portal or iHanoi, and carries no payment or tax function.
- **Consequences:** Integration points with iHanoi, Hanoi Portal (context Flow-11) and the agency directory must be agreed with their owners; the manual-routing legacy is not carried over.
