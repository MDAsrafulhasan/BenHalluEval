# Raw inference outputs

Use `outputs/<run_id>/` for durable JSONL predictions and matching `.meta.json` checkpoint configurations. Use the same run ID under `experiments/` and `results/`.

Keep raw prompts/responses, IDs, usage, timing, parsed results and failure flags intact. Resume only with the same dataset and protocol. Do not edit JSONL to manufacture completeness or bypass configuration checks. The active harness uses per-checkpoint OS locks and fsync.

Mark synthetic runs clearly (for example `mock_smoke`). Real inference payloads can contain private or copyrighted text; review before sharing. Git ignores raw JSONL by default, which is neither encryption nor backup. Preserve sidecars and separately back up any outputs needed to reproduce the thesis.

This directory is separate from the Huntrix reference's preserved `output/` (reports/slides) and its externally generated `outputs/` runtime assets. They are not interchangeable.
