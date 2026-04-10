# anyone-can-patch

Workshop materials for **research → apply security fix → validate** using AI-assisted workflows (e.g. Cursor).

**Canonical repository:** [https://github.com/Root-IO-Labs/anyone-can-patch](https://github.com/Root-IO-Labs/anyone-can-patch)

## Layout

- **`anyone-can-patch/`** — Reusable prompts (`prompts/`) and skills (`skills/`). Treat as read-only when running a workshop; copy into per-CVE example folders as needed.
- **`examples/`** — Fully worked or partial examples (CVE-specific prompts, patches, `research.json`, `validation.json`). **`fix-tree/`** and **`target-tree/`** are gitignored here; clone them locally per the example’s workshop guide.

## Quick start

Clone this repo, open it in your editor, then follow an example’s `WORKSHOP-DEMO-GUIDE.md` (for instance `examples/CVE-2025-68675-apache-airflow-2.10.5/`).
