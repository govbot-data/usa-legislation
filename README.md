# 🏛️ United States Legislative Data (govbot)

This repository contains **normalized, versioned legislative data outputs** produced by the govbot civic data system. It is designed for filesystem-based exploration, reproducibility, and downstream analysis.

Most users will interact with this data via the **govbot CLI and GitHub Actions**. This repository documents the structure, guarantees, and conventions of the output rather than the full pipeline implementation.

---

## What This Repository Provides

Each state dataset is generated automatically and committed on a schedule. The output is:

* **Deterministic** – ephemeral fields are removed to ensure stable diffs
* **Incremental** – only new or updated items are processed
* **Auditable** – actions, votes, and events are preserved as append-only logs
* **Analysis-friendly** – structured JSON optimized for SQL and tooling such as DuckDB

---

## High-Level Pipeline (Context)

Govbot processes legislative data in two primary stages today:

1. **Ingest** – Scrape structured legislative metadata from Open States
2. **Format** – Normalize, link events, and write deterministic, versioned outputs to disk

This repository represents the **formatted output layer** of the system.

Govbot is intentionally conservative about derived data. While full-text extraction from source documents is an active area of exploration, it is not part of the public pipeline today due to accuracy and validation constraints, particularly around redlines and cross-outs in PDF documents.

---

## Repository Structure (Summary)

```
country:us/
  state:xx/
    sessions/
      {session_id}/
        bills/
          {bill_id}/
            metadata.json
            files/
            logs/
        events/
.windycivi/
```

* `metadata.json` – normalized bill metadata with processing timestamps
* `logs/` – append-only action, vote, and event logs
* `files/` – original source documents when present
* `.windycivi/` – pipeline metadata, errors, and data quality tracking

---

## Data Quality & Reliability

The pipeline automatically tracks:

* **Orphaned bills** – votes or events without corresponding bill metadata
* **Incremental progress** – last-seen timestamps to prevent duplicate work
* **Processing errors** – categorized and committed for inspection

Pipelines are designed to be fault-tolerant. If a run fails or is interrupted, subsequent runs resume safely without reprocessing prior data.

---

## Using the Data

Most users will:

* Clone this repository
* Pull updates via `git pull`
* Traverse files directly or load JSON into analysis tools

Example: loading bill metadata into DuckDB via the govbot CLI:

```bash
govbot load
duckdb govbot.duckdb
```

For running pipelines, onboarding new states, or configuration details, see the main govbot documentation.

---

## Related Repositories

* **Main system & CLI**: [https://github.com/chihacknight/govbot](https://github.com/chihacknight/govbot)
* **State ingestion examples**: [https://github.com/govbot-openstates-scrapers](https://github.com/govbot-openstates-scrapers)

---

## About Windy Civi

This repository is part of the Windy Civi project, which builds open, verifiable infrastructure for long-term civic accountability through decentralized legislative data.
