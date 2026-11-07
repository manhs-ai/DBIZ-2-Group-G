# ADR-004 — LGSP data through daily batch sync

- **Status:** Proposed default (FR-02-04, UC-13, Flow-07/08, SQ-13/14)
- **Context:** Cross-verification needs government data from LGSP using a RAG-based lookup. Whether LGSP is read in real time or synchronised is unknown.
- **Decision:** Assume a **daily batch sync**, with queries run against the synced data (workbook default for open question #2).
- **Consequences:** Cheaper and less coupled than a real-time call, but cross-checks use data up to a day old. FR-02-04 is a Could after W4, so this affects only the full-scope scenario.
- **Open:** Confirm with the LGSP owner (blocking).
