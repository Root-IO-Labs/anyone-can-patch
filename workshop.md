# Workshop: CVE backport with Cursor

Clone this repo, open it in Cursor (**Agent** mode), then **cut and paste** prompts into chat. Read each helper prompt below before you paste it so you understand what you are asking for.

**Facilitator depth:** `examples/CVE-2025-68675-apache-airflow-2.10.5/WORKSHOP-DEMO-GUIDE.md`  
**Agent rules:** `AGENTS.md`  
**Optional (not used in these steps):** skills under `anyone-can-patch/skills/` — same workflow if you prefer `@` skills later.

---

## Prerequisites

- Cursor with **Agent** mode (terminal, network, file edits)
- `git`, Python 3.9+ / `pip`, network
- A few GB disk; ~30–45 minutes (Airflow clones + install take most of it)

If `pip install -e .` fails on `google-re2`, run this first, then retry install:

```
pip install 'google-re2>=1.0' --only-binary=:all:
```

---

## Step 0 — Clone this workshop repo and open it

```bash
git clone https://github.com/Root-IO-Labs/anyone-can-patch.git
cd anyone-can-patch
```

In Cursor: **File → Open Folder** → select this repo’s root (`anyone-can-patch`).

**Two clones, different jobs:**

1. **This repo** — you clone it now (Step 0).
2. **Apache Airflow** into `fix-tree/` and `target-tree/` — the **agent** clones those during research. You do not clone Airflow by hand.

---

## Workshop example folder (prompts are already filled)

Work in:

```
examples/CVE-2025-68675-apache-airflow-2.10.5/
```

For this live example, **`1-Research-Prompt.md`**, **`2-Apply-Fix-Prompt.md`**, and **`3-Validate-Prompt.md` are already filled** with CVE-2025-68675, apache-airflow, 2.10.5, and the correct `fix-tree` / `target-tree` paths. **Do not** rewrite the CVE/package/version for the default workshop.

- Your outputs go **next to those prompts** (`research.json`, patch file, `PATCH.md`, `validation.json`).
- **`reference/`** is the answer key — open to compare; **do not overwrite**.
- Do not `git commit` / `git push` the Airflow trees unless the facilitator says so.

How to paste a **phase** prompt: open the `.md` file → read it → copy **only the fenced block** → paste into a new Agent chat → send.

How to paste a **helper** prompt: copy the fenced block from **this** file (below) into chat.

---

## Step 1 — Orientation (helper, optional)

Paste into a new Agent chat:

```
You are helping me run the anyone-can-patch workshop in this workspace.

1. Read AGENTS.md and workshop.md
2. Summarize: where prompts live, where my outputs go, where reference/ is, and why fix-tree and target-tree names matter
3. Confirm whether fix-tree/ and target-tree/ already exist under examples/CVE-2025-68675-apache-airflow-2.10.5/
4. Remind me that the three phase prompts in that folder are already filled for CVE-2025-68675 — I should not rewrite CVE/package/version
5. Do NOT clone Airflow or start CVE research yet—report only
```

Same text lives in `anyone-can-patch/prompts/0-Bootstrap-Orientation.md`.

---

## Step 2 — Research

1. Open `examples/CVE-2025-68675-apache-airflow-2.10.5/1-Research-Prompt.md` and read it.
2. Copy its fenced prompt block into a **new** Agent chat and send.
3. Wait until the agent has written **`research.json`** next to the prompts and created **`fix-tree/`** and **`target-tree/`**.
4. In the editor, open `research.json` and expand both tree folders in the file tree (see **How to confirm** below). Do not continue if there are no real trees.

---

## Step 3 — Sync apply prompt from research (helper)

The apply prompt’s **CONTEXT FROM RESEARCH** section must match **your** `research.json`. Paste this helper so the agent updates the file for you (do not invent CONTEXT by hand):

```
Update the apply-fix prompt from my research output. Do not start patching yet.

1. Read examples/CVE-2025-68675-apache-airflow-2.10.5/research.json
2. Edit examples/CVE-2025-68675-apache-airflow-2.10.5/2-Apply-Fix-Prompt.md
3. Fill CONTEXT FROM RESEARCH (fixed version, fix commit SHAs/PRs, files to modify, complexity) from research.json
4. Keep CVE-2025-68675, apache-airflow, 2.10.5, branch patch-CVE-2025-68675, target-tree path, and patch filename CVE-2025-68675-target-tree.patch consistent
5. Prefer the minimal secrets_masker / DEFAULT_SENSITIVE_FIELDS backport for this workshop
6. Show me a short diff summary of what you changed in 2-Apply-Fix-Prompt.md, then stop
```

Same text lives in `anyone-can-patch/prompts/0-Sync-Apply-Context.md`.

Then open `2-Apply-Fix-Prompt.md` in the editor and skim CONTEXT before Step 4.

---

## Step 4 — Apply fix

1. Open `examples/CVE-2025-68675-apache-airflow-2.10.5/2-Apply-Fix-Prompt.md` and read the updated CONTEXT.
2. Copy its fenced prompt block into a **new** Agent chat and send.
3. Expect: branch `patch-CVE-2025-68675` on `target-tree`, edits under `target-tree/`, and `CVE-2025-68675-target-tree.patch` next to the prompts.
4. In the editor, open the patch file and `target-tree/airflow/utils/log/secrets_masker.py` (see **How to confirm**).

---

## Step 5 — Write PATCH.md

Paste into chat (same thread as apply or a new one):

```
Write examples/CVE-2025-68675-apache-airflow-2.10.5/PATCH.md next to the prompts (not under reference/).

Use my research.json, CVE-2025-68675-target-tree.patch, and the fix-tree / target-tree layout.
Explain the CVE in plain language, what the minimal backport does (DEFAULT_SENSITIVE_FIELDS / proxy and proxies), and what we did not include (e.g. broader PRs).
Do not overwrite reference/PATCH.md.
```

Open `PATCH.md` in the editor when done.

---

## Step 6 — Sync validate prompt from apply output (helper)

Paste:

```
Update the validate prompt from my apply-fix output. Do not start install/tests yet.

1. Inspect examples/CVE-2025-68675-apache-airflow-2.10.5/ (research.json, CVE-2025-68675-target-tree.patch, target-tree changes)
2. Edit examples/CVE-2025-68675-apache-airflow-2.10.5/3-Validate-Prompt.md
3. Fill CONTEXT: CVE, files modified, short fix summary, target-tree path, branch patch-CVE-2025-68675, patch filename, build command (pip install -e .), test command (pytest tests/utils/log/test_secrets_masker.py)
4. Keep OUTPUT json aligned with validation.json we will save next to the prompts
5. Show me a short diff summary of what you changed in 3-Validate-Prompt.md, then stop
```

Same text lives in `anyone-can-patch/prompts/0-Sync-Validate-Context.md`.

Open `3-Validate-Prompt.md` and skim CONTEXT before Step 7.

---

## Step 7 — Validate

1. Open `examples/CVE-2025-68675-apache-airflow-2.10.5/3-Validate-Prompt.md` and read it.
2. Copy its fenced prompt block into a **new** Agent chat and send.
3. Expect install + targeted tests in `target-tree/`, then **`validation.json`** next to the prompts.
4. Open `validation.json` in the editor (see **How to confirm**).

---

## How to confirm it worked

Use the Cursor file tree and open files in the editor. Don’t rely only on the chat summary.

Folder: `examples/CVE-2025-68675-apache-airflow-2.10.5/`  
Outputs sit **next to** the prompts. **`reference/`** is compare-only.

### After research

Open **`research.json`**. Check:

- `cve_id` / `package` / `vulnerable_version` match the exercise
- `recommendation` is `PROCEED` (or `CAUTION` you accept)
- `fix_commits` includes PR #61906 / SHA starting `a260fb7`
- complexity looks **LOW**

Expand **`fix-tree/`** and **`target-tree/`** in the file tree so both are real folders.

Optional: open **`reference/research.json`** side by side.

### After apply

Open **`CVE-2025-68675-target-tree.patch`**. Look for:

- `+        "proxy",`
- `+        "proxies",`

Then open `target-tree/airflow/utils/log/secrets_masker.py` and confirm those names are in `DEFAULT_SENSITIVE_FIELDS`.

Optional: open **`reference/CVE-2025-68675-target-tree.patch`**.

### After PATCH.md

Open **`PATCH.md`**. Plain-language CVE, `secrets_masker.py` / `DEFAULT_SENSITIVE_FIELDS`, minimal backport.

### After validate

Open **`validation.json`**. Check:

- `build.success` is `true`
- secrets_masker tests mostly passing (**58 passed**, **1 xfailed** is expected)
- `verdict` is **`APPROVED`** (or `NEEDS_REVIEW` with notes you understand)
- `security_review.root_cause_addressed` is `true`

Optional: open **`reference/validation.json`**.

**Bottom line:** PROCEED → patch adds `proxy`/`proxies` → source matches → `validation.json` says APPROVED.

---

## Soft reset (helper)

If trees already exist and you need a clean research run, paste:

```
I will re-run research in examples/CVE-2025-68675-apache-airflow-2.10.5/.
If fix-tree/ or target-tree/ exist, summarize their HEAD refs; ask before deleting.
Do not modify reference/. Do not start NVD research until I say so.
```

---

## Practice folder (helper, optional)

Only if you want a second run without touching the shipped example. Paste:

```
Bootstrap a practice run of the Airflow CVE workshop without touching the answer-key example.

1. Create examples/CVE-2025-68675-apache-airflow-2.10.5-practice/
2. Copy 1-Research-Prompt.md, 2-Apply-Fix-Prompt.md, and 3-Validate-Prompt.md from anyone-can-patch/prompts/ into that folder
3. Fill CVE-2025-68675, apache-airflow, 2.10.5 everywhere; set fix-tree and target-tree paths under examples/CVE-2025-68675-apache-airflow-2.10.5-practice/
4. Do NOT copy reference/, fix-tree/, or target-tree/ from the original example
5. Stop and show the folder listing + a short “next: paste research prompt” note
```

Then repeat Steps 2–7 using the prompts under `…-practice/`.

Same text lives in `anyone-can-patch/prompts/0-Bootstrap-Practice-Folder.md`.

---

## Stuck?

- **No trees after research** — incomplete; re-run Step 2; do not accept API/raw-only (`AGENTS.md`).
- **Wrote into `reference/`** — move outputs to the example root; restore `reference/` from git if needed.
- **Wrong paths on apply** — re-run Step 3 sync, then Step 4.
- **`google-re2` / install** — use the pip tip under Prerequisites; or keep `pytest tests/utils/log/test_secrets_masker.py` as the focused check.
- **Lost / slow** — compare your files to `reference/` and continue; document the gap.

---

## Links

- NVD: https://nvd.nist.gov/vuln/detail/CVE-2025-68675  
- Upstream PR (minimal fix): https://github.com/apache/airflow/pull/61906  
- Kit: `anyone-can-patch/README.md`
