# ADR-003 — AI classification as a separate service behind the backend

- **Status:** Proposed (SQ-08, SQ-09, FR-02-02)
- **Context:** Classification and confidence scoring need an AI model whose behaviour changes independently of the business logic.
- **Decision:** The Alert Service calls an AI Service (Classification) and stores the returned category and confidence (SQ-08…10). The model is integrated, not built.
- **Consequences:** Integration is the most expensive row in F-02 (FR-02-02-INT, 22.4 p-h, vague EARS) and carries the highest risk order, so it is built first. Model availability, latency and fallback when the service is down are unspecified.
- **Open:** Which model or vendor; what happens to a report when the AI Service is unavailable.
