# Data lifecycle

- [`raw/benhallu/`](raw/benhallu/): original Track-A/Track-B data, preserved byte-for-byte.
- [`raw/dialect/`](raw/dialect/): reserved for a real, properly sourced dialect benchmark; none supplied yet.
- [`splits/`](splits/): generated canonical/mock evaluation subsets and their manifests.

Read [the data guide](../docs/DATA_GUIDE.md) before normalization or integration. Never write derived CSVs over raw inputs. Keep original text, IDs, provenance and licenses; hash datasets used by each experiment. Split by source lineage for any future training/development/calibration partition.

The raw summarization taxonomy requires an explicit reviewed mapping. Benchmark answers and generated hallucinations must not leak into independent judge evidence.
