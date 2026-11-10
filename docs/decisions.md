## ADR-001: Analytics store

_Date: 2026-11-08 · Status: Accepted_

**Context**

- Two kinds of data: Petrinex/AER CSVs (a few hundred MB, monthly loads,
  read-heavy aggregations) and directive PDFs stored as embedded chunks.
- The agent must be restricted to curated views, read-only.

**Options**

1. Postgres + pgvector (chosen)
2. CSV/Parquet in S3: easy to set up, but no way to restrict the agent to
   curated views, no vector search, and still needs a query engine.
3. DuckDB: columnar, fastest aggregations, near-zero ops; but embedded,
   with no roles/GRANTs to restrict the agent to views.

**Decision**
Postgres, for two reasons:

- Roles and GRANTs restrict the agent to SELECT on views only.
- pgvector keeps production tables and directive chunks in one database.

**Consequences**

- Aggregations may be slower than a columnar engine on large scans.
- Must operate a DB server: hosting, backups, schema migrations.
- Mitigation if needed: materialized views for common heavy aggregations.

**How we'll know**

- Metric: p95 query latency across the SQL eval suite.
- Threshold: p95 > 1.1 s → revisit DuckDB/Parquet. (10 s answer budget,
  ⅓ for SQL, up to 3 queries per answer.)
- Prediction: THe p95 should come under 1.1s.
