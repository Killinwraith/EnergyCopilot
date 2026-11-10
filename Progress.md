## Session 2026-11-08 (continued from 2026-11-05)

Milestone: M0 — Orientation and first decisions (~90%)
Built: Request-path walkthrough (online + offline); ADR-001 written into docs/decisions.md
Decisions logged: ADR-001 (analytics store: Postgres + pgvector; revisit if p95 > 1.1 s on SQL suite)
Eval results: none yet
Learned: The router's ability to pick the allowed tool list is itself evaluated, because a misroute means the agent never gets the tool it needs.
Shaky on:

- Prompt injection via retrieved text: defence is "retrieved text is data, not instructions" + safety suite (half right twice)
- Naming architecture trade-offs unprompted
- Grounding check ≠ correctness check (verifier passes a well-grounded wrong number)
- Top-k search can't answer "every"; requirements table + WHERE can
  Hints used: Rung 3 on semantic views as the guard against bad joins, on requirements table vs vector search, and on the router's gain
  Next: Add the reason to the ADR-001 prediction. Open next session with two trade-offs cold (M0 gate), then M1: "What happens if the loader runs twice for the same month?"
