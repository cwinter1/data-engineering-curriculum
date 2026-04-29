# Data Engineering Curriculum

A hands-on, project-driven curriculum for engineers who want to work in data — built from real production experience in Israeli fintech and eCommerce, not from textbooks.

This is not a course you can buy. It was assembled over years of building pipelines in banking, regulatory environments, and high-volume eCommerce — and refined through the process of teaching it. Every module is grounded in problems that actually occur at work.

---

## Who It's For

Data engineers who already know the fundamentals and want to stay relevant as the field shifts.

The target is practitioners with 2–5 years of experience who can build a pipeline but haven't yet worked with the tooling, patterns, and architectural approaches that are now standard in 2026 hiring. The curriculum doesn't start from zero — it builds on what you already know and fills the gaps that separate a working DE from a hireable one.

**This course is for you if:**
- You know Python and SQL but haven't worked with modern orchestration (Airflow), streaming (Kafka), or cloud-native storage (BigQuery, Athena)
- You've built pipelines but never had to harden them for production — security, schema evolution, incident response
- You're preparing for a role change and need to close the gap between your current stack and what Israeli companies are actually asking for in 2026
- You learn by building, not by watching videos or reading slides

**What you leave with:**
- A working capstone project that demonstrates end-to-end pipeline ownership
- Hands-on exposure to the tools and patterns appearing in 2026 JDs: Kafka, Airflow, dbt-style contracts, cloud IAM, CI/CD for data
- The ability to talk through architecture decisions and failure modes in an interview — not just describe what you built

Not for people who want a certificate. For people who want to be able to do the job.

---

## How It's Taught

**No slides. No multiple choice.**

Each module follows the same pattern:

1. **The problem first** — you see a broken or missing pipeline, a failing query, a production incident. You understand *why* this matters before you touch any code.
2. **Guided build** — you build a working solution from scratch, with the instructor walking through decisions in real time, not just showing the answer.
3. **Break it deliberately** — after it works, you introduce a failure: a schema change, a late partition, a Kafka consumer falling behind. You fix it yourself.
4. **Capstone application** — each module's techniques feed into a running capstone project. Nothing is isolated.

Learning is cumulative. Week 6 pipelines use the SQL from Week 2 and the orchestration from Week 4. If you skip a module, you feel it.

---

## Architecture

Each capstone is built iteratively, one layer per module. By the end of the curriculum the student has assembled this full stack from scratch:

```
[Source systems]
      │
      ▼
[Ingestion layer]          Python scripts, incremental fetch, idempotent writes
      │                    Module 1 (Python) + Module 3 (Pipeline Design)
      ▼
[Raw storage]              S3 / BigQuery raw dataset / PostgreSQL staging schema
      │                    Module 4 (Cloud Storage)
      ▼
[Transformation layer]     SQL CTEs, window functions, materialised views
      │                    Module 2 (SQL) — runs in Docker-based PostgreSQL locally
      ▼
[Orchestration]            Airflow DAGs — retries, sensors, SLA alerts, backfill
      │                    Module 5 (Orchestration)
      ▼
[Real-time feed]           Kafka producer → consumer → append to warehouse
      │                    Module 6 (Stream Processing)
      ▼
[Quality layer]            dbt tests as reference, custom data contracts, alerts
      │                    Module 7 (Testing & Quality)
      ▼
[Security hardening]       IAM, secrets, PII masking, audit log
      │                    Module 8 (Cybersecurity)
      ▼
[Monitoring & runbooks]    Logging, alerting, incident response
                           Module 9 (Production Patterns)
```

**Local environment** runs on Docker: one `docker-compose.yml` spins up PostgreSQL, a local Airflow instance, and a Kafka broker. Students work against real services from day one — no mocks, no in-memory simulators.

---

## Curriculum

| Module | Topics | Builds toward |
|--------|--------|--------------|
| 1 — Python for Data | File I/O, data structures, type hints, error handling, CLI design | Capstone ingestion layer |
| 2 — SQL That Scales | Window functions, CTEs, query plans, index strategy, materialised views | Capstone transformation layer |
| 3 — Pipeline Design | ETL vs ELT, idempotency, incremental loads, backfill patterns, late data | Capstone pipeline contract |
| 4 — Cloud Storage | S3, BigQuery, Athena — reading, writing, partitioning, cost awareness | Capstone storage layer |
| 5 — Orchestration | Airflow DAG design, retries, sensors, task dependencies, SLA alerts | Capstone scheduler |
| 6 — Stream Processing | Kafka concepts, consumer groups, offset management, at-least-once delivery | Capstone real-time feed |
| 7 — Testing & Quality | Unit tests, integration tests, data quality contracts | Capstone QA layer |
| 8 — Cybersecurity | Secrets management, IAM, encryption, PII handling, audit logging | Capstone hardening pass |
| 9 — Production Patterns | Logging, alerting, schema evolution, runbooks, incident response | Capstone production readiness |

---

## Capstone Projects

Each cohort builds one of two capstones end-to-end, from raw ingestion to a live dashboard. Both go through every module — nothing is skipped for the capstone.

**Capstone A — Fintech Transaction Pipeline**

Ingest, validate, and aggregate a stream of payment events into a reporting layer.

```
Bank API (mock)
    │
    ▼
Python ingestion script     incremental fetch, idempotent upsert
    │
    ▼
PostgreSQL (Docker)         raw events schema → aggregated daily totals
    │
    ▼
Airflow DAG                 scheduled nightly, retry on failure, SLA alert at 07:00
    │
    ▼
Kafka feed                  real-time fraud flag events piped into the warehouse
    │
    ▼
Security hardening          PII masking (card numbers, names), audit log, secrets vault
    │
    ▼
Monitoring                  pipeline health dashboard, runbook for common failures
```

Mirrors the architecture used in Israeli banking environments — including the regulatory audit trail requirement.

**Capstone B — eCommerce Analytics Platform**

Multi-source ingestion, incremental daily loads, dimensional model, self-service query layer.

```
Orders API + Inventory DB + Ad Spend CSV
    │
    ▼
Python ingestion (3 sources)   each source independent, failures isolated
    │
    ▼
S3 raw layer                   partitioned by date and source
    │
    ▼
BigQuery                       staging → dimensional model (orders_fact, products_dim)
    │
    ▼
Airflow DAG                    daily load, backfill support, data quality gate
    │
    ▼
Security hardening             IAM scoping, no plaintext credentials in code or CI
    │
    ▼
Self-service layer             parameterised SQL queries, documented schema
```

Based on patterns from a real 50-store Amazon operation — same volume, same failure modes.

---

## Failsafe Design

The curriculum itself is designed to be robust to student failure — the same way production pipelines are designed to be robust to data failure.

| Scenario | How it's handled |
|----------|-----------------|
| Student skips a module | Capstone layer for that module won't work; the gap is immediately visible |
| Docker environment broken | `docker-compose down -v && docker-compose up` resets to clean state; data rebuilt from scripts |
| Airflow DAG in broken state | Runbook exercise — diagnosing and clearing stuck DAG runs is part of Module 9 |
| Kafka consumer falls behind | Handled explicitly in Module 6 — offset reset, consumer group lag monitoring |
| Schema migration breaks pipeline | Module 9 covers backwards-compatible migrations and blue/green schema patterns |
| Cloud credentials expired | Module 8 — short-lived credential rotation, IAM role assumption |

The Docker-based local environment means every student starts from an identical baseline. "Works on my machine" is not a valid failure mode.

---

## QA Automation

Testing is woven into every module, not saved for Module 7.

**Module 1** — students write `unittest` tests for every ingestion function before implementing it (TDD from day one)

**Module 3** — pipeline contracts: input schema, output schema, row count assertions, null checks

**Module 7 (dedicated testing module)**

```python
# Data quality contract example
class TestDailyOrders(unittest.TestCase):

    def test_no_nulls_in_order_id(self):
        df = load_daily_orders(date="2026-04-29")
        self.assertEqual(df["order_id"].isnull().sum(), 0)

    def test_total_revenue_positive(self):
        df = load_daily_orders(date="2026-04-29")
        self.assertGreater(df["revenue"].sum(), 0)

    def test_row_count_within_expected_range(self):
        df = load_daily_orders(date="2026-04-29")
        self.assertBetween(len(df), 100, 100_000)
```

**Capstone sign-off checklist**
Before a capstone is signed off, it must pass:
- [ ] All ingestion functions have unit tests
- [ ] Pipeline is idempotent (re-run produces same result)
- [ ] Schema migration doesn't break existing queries
- [ ] No secrets in code, git history, or environment printouts
- [ ] Runbook covers the 3 most likely failure modes
- [ ] PII fields are masked in all non-production outputs

---

## Cybersecurity Module (Module 8)

Data engineers move sensitive data. Most don't think about what happens when something goes wrong.

This module covers:

- **Secrets management** — no credentials in code or git history; environment injection, Vault, AWS Secrets Manager
- **IAM least privilege** — S3 bucket policies, BigQuery dataset permissions, service account design
- **Encryption** — at-rest (S3 SSE, BigQuery CMEK) and in-transit (TLS, client cert validation)
- **PII in pipelines** — tokenisation, masking, pseudonymisation; when to apply each; GDPR implications for Israeli companies with EU customers
- **Audit logging** — what to log, where to ship it, how to detect anomalies
- **Incident response for data** — what to do when a pipeline leaks data, writes to the wrong table, or exposes a credential

This is not a security certification track. It's the minimum a production data engineer needs to not get their company breached.

---

## Stack

Python 3.10+, PostgreSQL (Docker), BigQuery, AWS (S3 / Athena / Glue), Apache Airflow (Docker), Apache Kafka (Docker), Spark (Module 6 intro), HashiCorp Vault (Module 8)

---

## What You Leave With

- A working capstone pipeline deployed to a real cloud environment
- The ability to read and write production-grade Python without hand-holding
- Enough security awareness to not be the person who commits an API key
- A mental model of how data moves through a real organization — not a demo environment

---

*Built from production experience. Not available for purchase. Taught in cohorts.*
