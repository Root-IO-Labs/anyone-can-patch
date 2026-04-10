# Todo (later)

## Enforce real git clones for CVE workflow (no API-only shortcuts)

### Background

This workspace uses **`anyone-can-patch`**: three prompts (research → apply fix → validate) for backporting security fixes to **vulnerable package versions** (e.g. PyPI/npm). The workshop material lives under `anyone-can-patch/`; per-example filled prompts, research output, and patches live under `examples/<CVE-package-version>/` (e.g. `examples/CVE-2025-68675-apache-airflow-2.10.5/`).

The **written** instructions are explicit about using **real source trees**, not only web lookups:

- **Research (`1-Research-Prompt.md` / `skills/research-cve.md`):** Phase 3 says to **clone the upstream repository** and check out the **fix commit(s)**; Phase 4 says to **clone the vulnerable version** and compare structure for backporting.
- **Apply fix (`2-Apply-Fix-Prompt.md` / `skills/patch-cve.md`):** Phase 1 says to **locate or clone** the **vulnerable** source, create a patch branch, then edit and emit a unified diff.

There are also **optimization** lines in research (e.g. tool-call budgets, “enough vs perfect” information). Those are easy to **over-interpret** as permission to skip heavy steps; they do **not** explicitly say “do not clone.”

### Preferred working layout (project convention)

Use **two separate working copies** of the **same upstream remote** (not one repo with constant checkout switching), for example:

| Clone / directory | Role | Typical ref |
|-------------------|------|-------------|
| **Fix tree** | Inspect upstream fix: `git show`, diffs, optional tests on the fixing side | Fix merge SHA or fixed release tag (e.g. `2.11.1`) |
| **Target tree** | Apply backport, branch `patch-CVE-…`, `pip install -e .`, `pytest` | Vulnerable tag (e.g. `2.10.5`) |

Same URL twice, two folders—keeps the reference fix tree clean and the patched vulnerable tree isolated for demos and validation.

### What went wrong in practice

Some agent runs completed **research** and **apply-fix** using the **GitHub API** and **`raw.githubusercontent.com`** to read files and diffs, without **`git clone`** of both refs first.

- That was an **implementation shortcut** by the agent, **not** something the prompts instruct.
- It can still produce a **plausible small patch** (e.g. a two-line `secrets_masker` change) but it **skips** the workflow the prompts describe and **does not** automatically leave a tree ready to **build and run tests** as validate expects.

### Why fixing this matters

- **Reproducibility:** Patches should apply cleanly to the **checked-out** vulnerable tree; `patch -p1` and `git diff` should match real state.
- **Validate phase:** `pip install -e .` and `pytest` require a **full** checkout (and often deps), not a single file fetched over HTTP.
- **Demos:** Showing two directories and `git rev-parse HEAD` in each is clearer than “we read the API.”

### Do later (action items)

- [ ] Add a **persistent rule** (Cursor **user rules**, or **`AGENTS.md`** / **`.cursor/rules`** in this repo) that states: for CVE research and apply-fix, **always** create/maintain **two local clones** at defined paths (fix ref + vulnerable ref) **before** claiming research complete or finalizing a patch; **do not** rely on API-only or raw-URL reads unless the user **explicitly** allows it or the environment cannot reach `git` (then **state that** in the reply).
- [ ] Optionally **edit** `anyone-can-patch/prompts/*.md` and `skills/*.md` with **one explicit sentence**: cloning/checkouts are **required** for the phases that name them; optimization rules **must not** replace those steps without user approval.
- [ ] For each **example** folder under `examples/`, add a short **`README` or `CLONES.md`**: suggested directory names (e.g. `upstream-fix/`, `upstream-vulnerable-2.10.5/`), exact `git clone` + `git checkout` commands, and a **verification** snippet (`git rev-parse HEAD`, `git status -sb`).

### Related files (reference)

- `anyone-can-patch/prompts/1-Research-Prompt.md` — Phases 3–4 (clone fix + clone vulnerable).
- `anyone-can-patch/prompts/2-Apply-Fix-Prompt.md` — Phase 1 (clone/locate vulnerable).
- `anyone-can-patch/skills/research-cve.md`, `patch-cve.md` — same phases in skill form.
- Example work in progress: `examples/CVE-2025-68675-apache-airflow-2.10.5/` (research JSON, filled prompts, patch file).
