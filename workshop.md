# Workshop: CVE backport with Cursor

Participant quickstart. You start by cloning **this** repository, open it in Cursor, then paste three prompts (with skills) to research → apply → validate a security fix.

**Facilitator depth / talk-track:** see `examples/CVE-2025-68675-apache-airflow-2.10.5/WORKSHOP-DEMO-GUIDE.md`.  
**Agent rules:** see `AGENTS.md` at the repo root.

---

## Prerequisites

- [Cursor](https://cursor.com/) with **Agent** mode (needs terminal / network / file edits)
- `git` on your PATH
- Python 3.9+ and `pip` (3.10+ preferred; for the Airflow validate step)
- Network access (GitHub, NVD, `pip`)
- Disk: a few GB free (Airflow clones + install)
- Time: ~30–45 minutes for the default example (Airflow shallow clones ~2–5+ min; `pip install -e .` can take longer)

**Validate tip:** If `pip install -e .` fails building `google-re2` from source, run  
`pip install 'google-re2>=1.0' --only-binary=:all:`  
then retry the editable install.

---

## Step 0 — Clone this workshop repo and open it

```bash
git clone https://github.com/Root-IO-Labs/anyone-can-patch.git
cd anyone-can-patch
```

In Cursor: **File → Open Folder** and select the `anyone-can-patch` directory (the repo root). Paths in the prompts and `@` skills assume that root.

Optional orientation (paste into a new Agent chat):

```
You are helping me run the anyone-can-patch workshop in this workspace.

1. Read AGENTS.md and workshop.md
2. Summarize: where prompts live, where my outputs go, where reference/ is, and why fix-tree and target-tree names matter
3. Confirm whether fix-tree/ and target-tree/ already exist under examples/CVE-2025-68675-apache-airflow-2.10.5/
4. Do NOT clone Airflow or start CVE research yet—report only
```

Or `@` `anyone-can-patch/prompts/0-Bootstrap-Orientation.md` and paste the fenced block from that file.

---

## Two kinds of “clone” (read once)

| What | Who | When |
|------|-----|------|
| **This workshop repo** (`anyone-can-patch`) | You | Step 0 (now) |
| **Upstream package** (`fix-tree` + `target-tree`) | The agent | During **research**, after it discovers the upstream URL and refs |

There is **no** separate workshop step where you manually clone Apache Airflow. The research prompt/skill does that into directories named exactly **`fix-tree`** (fixed ref) and **`target-tree`** (vulnerable version).

---

## Default path — shipped Airflow example

Use the filled example (no need to invent a new CVE folder):

`examples/CVE-2025-68675-apache-airflow-2.10.5/`

| Role | Path |
|------|------|
| Copy-paste prompts | `1-Research-Prompt.md`, `2-Apply-Fix-Prompt.md`, `3-Validate-Prompt.md` |
| Skills to `@` | `anyone-can-patch/skills/research-cve.md`, `patch-cve.md`, `validate-cve.md` |
| Answer key (compare only) | `reference/` — **do not overwrite** |
| Your outputs | **Next to the prompts** in that example folder (`research.json`, patch file, `PATCH.md`, `validation.json`) |
| Upstream trees (gitignored) | `fix-tree/`, `target-tree/` — created by research |

### How to run each phase in Cursor

1. Open the prompt file for that phase; skim the “Before you copy-paste” notes.
2. Start a chat in **Agent** mode.
3. `@` the matching skill under `anyone-can-patch/skills/`.
4. Copy **only the fenced prompt block** into the chat and send.
5. Save artifacts where the prompt says (example folder root, not `reference/`).

| Phase | Prompt file | `@` skill | You should get |
|-------|-------------|-----------|----------------|
| Research | `1-Research-Prompt.md` | `research-cve.md` | `research.json` + `fix-tree/` + `target-tree/` (`git rev-parse HEAD` in each) |
| Apply fix | `2-Apply-Fix-Prompt.md` | `patch-cve.md` | Edits under `target-tree/` on `patch-CVE-2025-68675` + `CVE-2025-68675-target-tree.patch` |
| Explain | (ask in chat) | — | `PATCH.md` next to the prompts |
| Validate | `3-Validate-Prompt.md` | `validate-cve.md` | Install + tests in `target-tree/` → `validation.json` |

**Between research and apply:** fill **CONTEXT FROM RESEARCH** in `2-Apply-Fix-Prompt.md` from **your** `research.json`, then paste.

**Between apply and validate:** align CONTEXT in `3-Validate-Prompt.md` with the files/branch/patch you actually produced.

Workshop norm: **no `git commit` / `git push`** of the Airflow trees unless the facilitator says otherwise.

---

## Practice path — fresh folder (optional)

Use this if you want a clean run without reusing trees under the shipped example (e.g. facilitator dry-run). Keep the original example and `reference/` intact.

Paste (or `@` `anyone-can-patch/prompts/0-Bootstrap-Practice-Folder.md`):

```
Bootstrap a practice run of the Airflow CVE workshop without touching the answer-key example.

1. Create examples/CVE-2025-68675-apache-airflow-2.10.5-practice/
2. Copy 1-Research-Prompt.md, 2-Apply-Fix-Prompt.md, and 3-Validate-Prompt.md from anyone-can-patch/prompts/ into that folder
3. Fill CVE-2025-68675, apache-airflow, 2.10.5 everywhere; set fix-tree and target-tree paths under …-practice/
4. Do NOT copy reference/, fix-tree/, or target-tree/ from the original example
5. Stop and show the folder listing + a short “next: paste research prompt” note
```

Then run the same four phases, but use the prompts under `…-practice/` and save outputs there.

---

## Soft reset (reusing the shipped example)

If `fix-tree/` or `target-tree/` already exist and you need a clean research run:

```
I will re-run research in examples/CVE-2025-68675-apache-airflow-2.10.5/.
If fix-tree/ or target-tree/ exist, summarize their HEAD refs; ask before deleting.
Do not modify reference/. Do not start NVD research until I say so.
```

---

## Done checklist

- [ ] `research.json` at example root (not under `reference/`)
- [ ] `fix-tree/` and `target-tree/` present; HEADs recorded
- [ ] `target-tree` on `patch-CVE-2025-68675` with `proxy` / `proxies` in `DEFAULT_SENSITIVE_FIELDS` (`airflow/utils/log/secrets_masker.py`)
- [ ] `CVE-2025-68675-target-tree.patch` at example root matches `git diff`
- [ ] `PATCH.md` and `validation.json` at example root
- [ ] (Optional) Compared to `reference/` when stuck

---

## How to confirm it worked

Use the Cursor file tree and open files in the editor. Don’t rely only on the chat summary.

Example folder: `examples/CVE-2025-68675-apache-airflow-2.10.5/`  
(or `…-practice/` if you used the practice path). Your outputs sit **next to** the prompt files. **`reference/`** is the answer key—open it to compare, don’t overwrite it.

### 1. Research — `research.json` and the two trees

Open **`research.json`**. Confirm:

- `cve_id`, `package`, and `vulnerable_version` match the exercise
- `recommendation` is `PROCEED` (or `CAUTION` with a reason you accept)
- `fix_commits` includes the secrets-masker fix (PR #61906 / SHA starting with `a260fb7`)
- complexity looks **LOW** for this CVE

In the file tree, expand **`fix-tree/`** and **`target-tree/`** so both exist as real folders (not only links in chat).

Optional: open **`reference/research.json`** side by side—SHAs and recommendation should line up even if wording differs.

### 2. Apply — patch file and the changed source

Open **`CVE-2025-68675-target-tree.patch`**. Look for the two added lines:

- `+        "proxy",`
- `+        "proxies",`

under `DEFAULT_SENSITIVE_FIELDS` in `airflow/utils/log/secrets_masker.py`.

Then open the real file:

`target-tree/airflow/utils/log/secrets_masker.py`

Find `DEFAULT_SENSITIVE_FIELDS` and confirm **`proxy`** and **`proxies`** are in the frozenset (near `password` / `private_key` / `secret`).

Optional: open **`reference/CVE-2025-68675-target-tree.patch`** and compare—same two additions.

### 3. Explain — `PATCH.md`

Open **`PATCH.md`**. It should explain the CVE in plain language, name `secrets_masker.py` / `DEFAULT_SENSITIVE_FIELDS`, and describe this as the **minimal** backport.

### 4. Validate — `validation.json`

Open **`validation.json`**. Confirm:

- `build.success` is `true`
- `regression_tests` reports the secrets_masker tests mostly passing (**58 passed** and **1 xfailed** is expected for this example)
- `verdict` is **`APPROVED`** (or `NEEDS_REVIEW` with notes you understand)
- `security_review.root_cause_addressed` is `true`

Optional: open **`reference/validation.json`** and compare verdicts and test counts.

**Bottom line:** PROCEED → patch shows `proxy`/`proxies` → source file matches → `validation.json` says APPROVED means the run worked.

---

## Stuck?

| Symptom | What to do |
|---------|------------|
| Research finished with no local trees | Incomplete — require real clones + `git rev-parse HEAD` (`AGENTS.md`). Re-run research; do not accept API/raw-only. |
| Agent wrote into `reference/` | Move your files to the example root; restore `reference/` from git if needed. |
| Apply changed the wrong paths | Confirm CONTEXT and `target-tree` path match your example folder. |
| `pip install` / `google-re2` issues | Prefetch the wheel (`pip install 'google-re2>=1.0' --only-binary=:all:`), then `pip install -e .`. Or run targeted tests: `pytest tests/utils/log/test_secrets_masker.py` from `target-tree`. |
| Lost / too slow | Compare your artifacts to `reference/` and continue to the next phase; document the gap. |

---

## Reference links (default CVE)

- NVD: https://nvd.nist.gov/vuln/detail/CVE-2025-68675  
- Upstream PR (minimal fix): https://github.com/apache/airflow/pull/61906  
- Kit README: `anyone-can-patch/README.md`
