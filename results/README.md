# Derived evaluation reports

Use `results/<run_id>/` matching the experiment record and raw `outputs/<run_id>/` checkpoint directory. The evaluator writes CSV/Markdown tables and a report manifest; regenerate these from raw predictions rather than editing numbers manually.

Keep incomplete/undefined groups, parse failures and valid-pair coverage visible. BHS is lower-is-better error; CCC is agreement; CBS states its anchor and units. Report the 9-weight sensitivity sweep and paired CoT comparison without assuming a favorable outcome.

Only promote reviewed, validated results into `writing/`. Mock runs are synthetic pipeline checks and must never be cited as model performance. See [the workflow](../docs/WORKFLOW.md).
