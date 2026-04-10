# Agent instructions (CVE backport workshop)

This repository holds **`anyone-can-patch/`** (generic prompts + skills) and **`examples/`** (per-CVE filled prompts and reference outputs). When helping with **research → apply fix → validate** for a CVE, follow these rules in addition to any pasted prompt.

## Two real git checkouts are required for research (Phases 3–4)

1. **After** Phase 2 identifies the **upstream repository URL** and the **ref that contains the fix** (commit, tag, or release), you must **`git clone`** and check out that ref in a **dedicated working tree** (e.g. `fix-tree` when the user’s example folder defines that name). Use this tree for fix analysis (`git show`, `git diff`, file inspection)—not a substitute built only from the **GitHub API** or **raw.githubusercontent.com** URLs.

2. You must **`git clone`** the **same upstream** again into a **second** working tree at the **vulnerable version** (e.g. `target-tree`). Backport assessment compares real paths and symbols on disk. **Two directories, two refs**—do not satisfy Phase 4 by reusing one clone with repeated checkout churn unless the user explicitly asks for that layout.

3. **Do not** mark research complete or assert that Phases 3–4 are done if you only inspected remote files without these checkouts. If **`git` is unavailable** (sandbox, network), **say so explicitly** and list what was skipped.

4. Before moving on, provide evidence such as **`git rev-parse HEAD`** (and tag or describe if useful) **in each** tree so the user can verify refs.

## Optimization rules do not waive clones

Prompts may include tool budgets or “enough vs perfect” guidance. That **never** means you may skip **Phase 3–4 `git clone` steps** in favor of API-only or single-file HTTP reads, unless the **user explicitly** allows that shortcut.

## Apply-fix and validate

- **Apply-fix:** Prefer the **vulnerable source checkout** already created during research (e.g. `target-tree`). Do not replace it with patching from API-fetched snippets alone when a full tree exists or can be cloned.
- **Validate:** **`pip install` / `npm install`** and tests require a **full** source checkout, not a one-off downloaded file.

## Examples layout

Under **`examples/<CVE-…>/`**, **`fix-tree/`** and **`target-tree/`** are typically **gitignored**; they are created locally per the example **`WORKSHOP-DEMO-GUIDE.md`** and filled prompts. **`reference/`** holds completed JSON/patch/docs for comparison only.
