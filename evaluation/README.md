# Active thesis evaluation suite

These are the original three evaluation scripts, moved together without changing their bytes:

- [make_splits.py](make_splits.py): validated canonical inputs, raw normalization, deterministic stratification, synthetic generators, manifests.
- [eval.py](eval.py): metrics, parsing/collapse detection, locked decoding, durable/resumable inference, reports.
- [test_pipeline.py](test_pipeline.py): standalone pytest verification, offline fixtures, HTTP mocks, checkpoint and report checks.

Run commands below **from the workspace root**, not from an imported repository. Script docstrings containing short `python eval.py`/`python make_splits.py` examples predate this relocation; use the root-relative paths shown here. Default output paths are relative to your current working directory.

## Verify

```sh
python evaluation/test_pipeline.py -q
python -m pytest -q
```

Root pytest configuration restricts default collection to this suite; imported reference tests are not silently included.

## BenHallu offline smoke test

```sh
python evaluation/make_splits.py --generate_mock_data --mode benhallu --output_dir data/splits/mock_benhallu_seed42
python evaluation/eval.py --input_csv data/splits/mock_benhallu_seed42/benhallu_micro_debug_120.csv --backend always_yes --model_name always_yes --prompt_mode both --output_dir outputs/mock_smoke --results_dir results/mock_smoke
python evaluation/eval.py --reports_only --output_dir outputs/mock_smoke --results_dir results/mock_smoke
```

The two degenerate binary mock judges should produce 50% symmetric BHS when both tracks are present. Mock outputs are pipeline checks, not model-quality results.

## Dialect offline smoke test

```sh
python evaluation/make_splits.py --generate_mock_data --mode dialect --output_dir data/splits/mock_dialect_seed42
python evaluation/eval.py --mode dialect --input_csv data/splits/mock_dialect_seed42/dialect_ccc_subset_300.csv --backend mock_score --model_name mock_constant_score --prompt_mode both --output_dir outputs/mock_dialect --results_dir results/mock_dialect
```

The mock score judge is constant; do not expect it to show useful agreement or dialect robustness. Real dialect data and reviewed ratings have not been supplied.

## Prepare real data

Read [DATA_GUIDE.md](../docs/DATA_GUIDE.md) before converting raw files. `--input_csv` accepts a canonical CSV or, for BenHallu, `data/raw/benhallu`. Raw summarization ingestion additionally requires `--type_mapping_json` pointing to an actual reviewed mapping. That decision has deliberately not been invented.

Inspect exact available options without executing inference:

```sh
python evaluation/make_splits.py --help
python evaluation/eval.py --help
```

## Real inference contract

Use `--backend openai` for an OpenAI-compatible chat-completions endpoint, `--base_url` for its API base, `--model_name` for the actual model, and `--api_key_env` for an environment-variable name. Do not paste credentials into commands or source files. Check privacy/cost policy before sending benchmark text remotely. A local compatible endpoint avoids remote data transfer but still needs its own model/runtime setup.

The harness hard-defaults to temperature `0.0`, maximum completion tokens `1024`, and seed `42`. Provider/hardware changes can still affect reproducibility. Unsupported endpoint parameters fail rather than silently changing the protocol.

Use a fresh `outputs/<run_id>/` and matching `results/<run_id>/` for each protocol. Resume with the same input, script version, dependencies, model/backend, seed and prompt mode. Configuration mismatches are rejected. JSONL responses are flushed and fsynced before the next request; an interrupted final write is recoverable. An already-issued remote request can still repeat after a crash before its response is durably saved.

## Report interpretation

- Table 1: overall/per-task BHS, track-specific errors, collapse, failures, latency.
- Table 2: all 12 fine-grained Track B types; missing denominators stay undefined.
- Table 3: paired-ID `BHS_CoT - BHS_ZeroShot`; **negative means improvement**.
- Appendix G: nine deployment-cost weight pairs; rankings/stability are measured only for compatible complete runs.
- Tables 4–5: dialect CCC and anchor-minus-Chittagong CCC gap (CBS).
- Manifests: configuration, dataset/checkpoint hashes, coverage, and interpretation policies.

Operational BHS counts failed parses as per-track errors; valid-only BHS and parse-failure rates remain explicit. CCC uses valid paired scores with coverage and leaves zero-denominator agreement undefined, without an arbitrary epsilon.
