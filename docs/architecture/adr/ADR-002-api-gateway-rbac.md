# ADR-002 — Single API gateway with role-based access control

- **Status:** Proposed (FR-05-01, FR-05-02, SQ-03)
- **Context:** Four kinds of actors (citizens, police, HPC/government staff, technology partners) call the module, and partners need integration endpoints.
- **Decision:** All traffic enters through one API gateway that authenticates the caller and forwards to the Alert Service; the backend enforces role-based permission checks on every endpoint.
- **Consequences:** Gateway and partner authentication are costed as INT (22.4 p-h); RBAC as BE (13.6 p-h). Both are NC rows (0 function points); FR-05-01 is the most expensive L2 function (29.2 p-h). Every new endpoint must carry a permission check.
- **Note:** Combined FR-05-01 + FR-05-02 = 42.8 p-h (see [w2-cost.md](../w2-cost.md)).
