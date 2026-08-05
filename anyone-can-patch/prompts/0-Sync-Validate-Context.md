## Helper: Sync validate prompt from apply output

**When to use:** After apply-fix has written the patch and edited `target-tree`, before you paste `3-Validate-Prompt.md`.

**Copy-paste this prompt:**

```
Update the validate prompt from my apply-fix output. Do not start install/tests yet.

1. Inspect examples/CVE-2025-68675-apache-airflow-2.10.5/ (research.json, CVE-2025-68675-target-tree.patch, target-tree changes)
2. Edit examples/CVE-2025-68675-apache-airflow-2.10.5/3-Validate-Prompt.md
3. Fill CONTEXT: CVE, files modified, short fix summary, target-tree path, branch patch-CVE-2025-68675, patch filename, build command (pip install -e .), test command (pytest tests/utils/log/test_secrets_masker.py)
4. Keep OUTPUT json aligned with validation.json we will save next to the prompts
5. Show me a short diff summary of what you changed in 3-Validate-Prompt.md, then stop
```

For a practice folder, change the example path to `…-practice/` in every line above.
