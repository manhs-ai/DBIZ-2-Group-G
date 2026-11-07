# ADR-001 — Route by AI confidence threshold of 0.9

- **Status:** Proposed (reconstructed from FR-02-03, FR-02-05, UC-08/09, SQ-11/16)
- **Context:** Manual classification caused unclear routes and slow responses (CVCA before state). The AI returns a category and a confidence score.
- **Decision:** Confidence **> 0.9** → auto-route to the specialised agency under HPC. **0.9 or below** → cross-check with LGSP; if the category is not confirmed, flag for human verification, then route via Hanoi Portal.
- **Consequences:** Needs a routing rule engine (FR-02-03-BE, 16 h) and a human-review queue (FR-02-05-BE). The boundary is inclusive on the manual side; FR-02-05 was corrected in W3 to match. No reviewer screen is costed.
- **Open:** Where does 0.9 come from, a policy value or the model default? (Open question #1, blocking.) Make it a configuration value, not a constant.
