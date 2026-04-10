# anyone-can-patch

Workshop materials for **research → apply security fix → validate** using AI-assisted workflows (e.g. Cursor).

**Canonical repository:** [https://github.com/Root-IO-Labs/anyone-can-patch](https://github.com/Root-IO-Labs/anyone-can-patch)

## Layout

- **`anyone-can-patch/`** — Reusable prompts (`prompts/`) and skills (`skills/`). Treat as read-only when running a workshop; copy into per-CVE example folders as needed.
- **`examples/`** — CVE-specific **filled prompts** plus **`reference/`** (completed `research.json`, patch, `PATCH.md`, `validation.json` for comparison). **`fix-tree/`** and **`target-tree/`** are gitignored; clone them per each example’s **`WORKSHOP-DEMO-GUIDE.md`**. Your own run’s outputs go **next to the prompts**, not inside **`reference/`**.

## Quick start

Clone this repo, open it in your editor, then follow an example’s `WORKSHOP-DEMO-GUIDE.md` (for instance `examples/CVE-2025-68675-apache-airflow-2.10.5/`).

**Cursor / agents:** read **`AGENTS.md`** at the repo root for non-negotiable rules (two git checkouts for research, no API-only shortcuts for Phases 3–4).
