# Bengali LLM Hallucination Detection & Dialectal Robustness

**Start here.** This workspace separates original research material, your active evaluation code, immutable data, experiments, and thesis writing. Nothing from the imported projects was discarded, flattened, or silently merged into the thesis pipeline.

## Find the right material

| I need to… | Start here |
|---|---|
| Find any local file, including notebooks, PDFs, code, figures, and releases | [Complete file index](docs/catalog/FILE_INDEX.md) · [CSV catalog](docs/catalog/file_catalog.csv) |
| Decide which research component to reuse safely | [Research reuse guide](docs/RESEARCH_REUSE.md) |
| Understand datasets, label conventions, and missing inputs | [Data guide](docs/DATA_GUIDE.md) |
| Run a reproducible experiment and prepare thesis evidence | [Workflow](docs/WORKFLOW.md) · [Experiment template](experiments/TEMPLATE.md) |
| Read the CUET_AL_Masaar project | [Original README](references/IUT_DATATHON_HALLU-main/README.md) · [Actual inference notebook](references/IUT_DATATHON_HALLU-main/CUET_AL_Masaar_inference_notebook.ipynb) |
| Read the Huntrix project | [Original README](references/IUTCSFESTDATATHON-main/README.md) · [v91 runbook](references/IUTCSFESTDATATHON-main/release/phase2-v91-public-submission/RUNBOOK.md) |
| Run the thesis evaluation suite | [Evaluation guide](evaluation/README.md) · [Pipeline & optimization audit](docs/AUDIT_REPORT.md) |
| Check verified bugs and limitations before real inference or reuse | [Code review](docs/CODE_REVIEW.md) · [Audit report](docs/AUDIT_REPORT.md) |
| Check that every original file survived the reorganization | `python tools/workspace.py verify` |

## Workspace layout

```text
Thesis/
├── README.md                  Start here
├── AGENTS.md                  Persistent instructions for future coding assistants
├── evaluation/                Active thesis splitter, evaluator, and tests
├── data/
│   ├── raw/benhallu/          Original Track-A / Track-B CSVs; do not edit
│   ├── raw/dialect/           Destination for the separately sourced dialect benchmark
│   └── splits/                Generated, seed-specific evaluation subsets
├── references/                Both imported repositories, internally unchanged
├── docs/
│   ├── RESEARCH_REUSE.md      Asset map, scientific limitations, reuse boundaries
│   ├── DATA_GUIDE.md          Schemas, label inversion, taxonomy decisions
│   ├── WORKFLOW.md            Reproducible research and writing workflow
│   └── catalog/              Searchable inventory + original-file fingerprints
├── environment/               Evaluation dependencies; isolated from GPU reference stacks
├── experiments/               Protocols, run records, decisions, adaptations
├── outputs/                   Durable raw prediction checkpoints
├── results/                   Derived tables and report manifests
├── writing/                   Your chapters, bibliography notes, figures, and tables
└── tools/workspace.py         Standard-library catalog, search, integrity verification
```

## First checks

Read the [code review findings](docs/CODE_REVIEW.md) and [evaluation pipeline & optimization audit](docs/AUDIT_REPORT.md) before real-model inference. The active pipeline review identifies current blocker status, transport/parsing hardening, and runtime benchmark results. Original reference code also has documented defects.

Run these commands **from the `Thesis` directory** using your evaluation Python environment:

```sh
python tools/workspace.py verify
python evaluation/test_pipeline.py -q
python tools/workspace.py find "judge"
python tools/workspace.py find "normaliz"
```

The reference projects require additional assets and GPU/software environments. The commands above do **not** run their notebooks, install GPU packages, contact an API, or download datasets.

### Offline pipeline smoke test

```sh
python evaluation/make_splits.py --generate_mock_data --mode benhallu --output_dir data/splits/mock_benhallu_seed42
python evaluation/eval.py --input_csv data/splits/mock_benhallu_seed42/benhallu_micro_debug_120.csv --backend always_yes --model_name always_yes --prompt_mode both --output_dir outputs/mock_smoke --results_dir results/mock_smoke
```

These are **synthetic plumbing results, not thesis findings**. See the [evaluation guide](evaluation/README.md) for dialect checks and real-data preparation.

## Non-negotiable scientific safeguards

1. **Labels are inverted between projects.** Thesis: `1 = hallucinated`, `0 = non-hallucinated`. Both reference projects: `0 = hallucinated`, `1 = faithful`. Validate the source schema before any explicit `1 - label` conversion.
2. **Independent evaluation is not benchmark-answer lookup.** Both references use benchmark-derived answer keys. Keep lookup-assisted reproduction separate from independent BenHalluEval evaluation. Do not expose test answers through retrieval, rules, calibration, or few-shot examples.
3. **Metrics are not interchangeable.** Thesis BHS is an error percentage, **lower is better**; CCC is agreement, higher is better. A competition F1 or leaderboard result is not a BHS/CCC result.
4. **Preserve provenance and licenses.** Imported code is attributed to its authors. A repository's code license does not automatically license datasets, models, figures, or third-party components.
5. **The three raw summarization labels still need an explicit taxonomy decision.** The splitter requires a reviewed mapping; no speculative mapping has been added.
6. **The dialect dataset has not been supplied.** Its folder contains instructions, not fabricated observations.

## Keep this workspace useful

- Search before implementing: `python tools/workspace.py find "QUERY"`.
- Add original work outside `references/` and `data/raw/`; record source paths and changes in an experiment record.
- After adding or moving working files, rebuild the catalog: `python tools/workspace.py catalog`.
- Verify originals whenever needed: `python tools/workspace.py verify`. **Do not refresh the original-file baseline to conceal changes.**
- A move is not a backup. Keep a separate backup of this folder, particularly raw data, experiment records, and thesis writing.

### Original → current locations

| Original location | Current location |
|---|---|
| `All data/` | `data/raw/benhallu/` |
| `IUT_DATATHON_HALLU-main/` | `references/IUT_DATATHON_HALLU-main/` |
| `IUTCSFESTDATATHON-main/` | `references/IUTCSFESTDATATHON-main/` |
| `make_splits.py`, `eval.py`, `test_pipeline.py` | `evaluation/` |

The pre-move [preservation manifest](docs/catalog/original_files.json) records **294 original files / 40,585,529 bytes**, their old and new paths, sizes, and SHA-256 hashes. The three evaluation scripts and both reference repositories were moved without modifying their bytes. Internal reference paths and their existing `output/`, `release/`, and `archive/` folders remain intact.
