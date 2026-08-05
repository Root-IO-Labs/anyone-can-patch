## Prompt 0b: Bootstrap a practice example folder

**When to use:** You want a clean practice run without touching the shipped answer-key example (`examples/CVE-2025-68675-apache-airflow-2.10.5/` and its `reference/`).

**Copy-paste this prompt:**

```
Bootstrap a practice run of the Airflow CVE workshop without touching the answer-key example.

1. Create examples/CVE-2025-68675-apache-airflow-2.10.5-practice/
2. Copy 1-Research-Prompt.md, 2-Apply-Fix-Prompt.md, and 3-Validate-Prompt.md from anyone-can-patch/prompts/ into that folder
3. Fill CVE-2025-68675, apache-airflow, 2.10.5 everywhere; set fix-tree and target-tree paths under examples/CVE-2025-68675-apache-airflow-2.10.5-practice/
4. Do NOT copy reference/, fix-tree/, or target-tree/ from the original example
5. Stop and show the folder listing + a short “next: paste research prompt” note
```

After this, run research → apply → validate using the prompts in the practice folder (see `workshop.md`).
