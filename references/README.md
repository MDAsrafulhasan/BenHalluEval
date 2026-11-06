# Preserved research references

Both directories below are original research imports. Their complete internal trees—including duplicate release copies, notebook outputs, historical notebooks, reports, figures, licenses, and scoped instructions—are preserved.

| Project | Entry point | Best next stop |
|---|---|---|
| CUET_AL_Masaar | [Original README](IUT_DATATHON_HALLU-main/README.md) | [Inference notebook](IUT_DATATHON_HALLU-main/CUET_AL_Masaar_inference_notebook.ipynb) · [Report](IUT_DATATHON_HALLU-main/CUET_AL_Masaar_report.pdf) |
| Huntrix | [Original README](IUTCSFESTDATATHON-main/README.md) | [v91 runbook](IUTCSFESTDATATHON-main/release/phase2-v91-public-submission/RUNBOOK.md) · [Technical paper](IUTCSFESTDATATHON-main/docs/TECHNICAL_PAPER.md) |

Before adapting anything, read the workspace [research reuse guide](../docs/RESEARCH_REUSE.md), the source license, relevant implementation, and tests. Imported scores and reproduction claims are author-reported until independently reproduced. Some original documentation describes superseded files or unavailable external assets; it has been retained as historical evidence rather than rewritten.

## Preservation rules

- Do not reorganize inside these trees, remove apparent duplicates, strip notebook outputs, or overwrite historical artifacts.
- Treat the imports as read-only. Write adaptations into the active thesis pipeline or a named experiment, with source paths, authors, license notices, changed behavior, and tests.
- Do not run all notebooks or install reference requirements merely to inspect them.
- A local file's presence does not grant redistribution rights for all datasets, figures, models, or third-party code.
- Both projects use `0 = hallucinated`, opposite to the active thesis pipeline. Their benchmark-answer lookups must not enter independent BenHalluEval evaluation.

The [complete index](../docs/catalog/FILE_INDEX.md) includes every imported file. SHA-256 verification is available with `python tools/workspace.py verify` from the workspace root.
