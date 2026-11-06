# Generated evaluation subsets

Generate into dataset-/seed-specific subdirectories, for example `mock_benhallu_seed42/` and `mock_dialect_seed42/`. Real canonical subsets should use a different clearly named directory and retain the splitter's manifest.

Run from the workspace root:

```sh
python evaluation/make_splits.py --generate_mock_data --mode benhallu --output_dir data/splits/mock_benhallu_seed42
python evaluation/make_splits.py --generate_mock_data --mode dialect --output_dir data/splits/mock_dialect_seed42
```

Micro, 10% and full BenHallu files are overlapping evaluation subsets—not train/dev/test partitions. Do not train or calibrate on one and call another an independent test set. See [the data guide](../../docs/DATA_GUIDE.md) for real-data preparation and taxonomy review.
