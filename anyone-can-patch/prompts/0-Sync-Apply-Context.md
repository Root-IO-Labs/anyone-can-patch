## Helper: Sync apply prompt from research.json

**When to use:** After research has written `research.json`, before you paste `2-Apply-Fix-Prompt.md`.

**Copy-paste this prompt:**

```
Update the apply-fix prompt from my research output. Do not start patching yet.

1. Read examples/CVE-2025-68675-apache-airflow-2.10.5/research.json
2. Edit examples/CVE-2025-68675-apache-airflow-2.10.5/2-Apply-Fix-Prompt.md
3. Fill CONTEXT FROM RESEARCH (fixed version, fix commit SHAs/PRs, files to modify, complexity) from research.json
4. Keep CVE-2025-68675, apache-airflow, 2.10.5, branch patch-CVE-2025-68675, target-tree path, and patch filename CVE-2025-68675-target-tree.patch consistent
5. Prefer the minimal secrets_masker / DEFAULT_SENSITIVE_FIELDS backport for this workshop
6. Show me a short diff summary of what you changed in 2-Apply-Fix-Prompt.md, then stop
```

For a practice folder, change the example path to `…-practice/` in every line above.
