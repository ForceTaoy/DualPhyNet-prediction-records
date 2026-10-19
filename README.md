# Bearing feature-fusion prediction records

Version 1.0.0 accompanies the manuscript **Mechanism-based feature fusion for bearing fault diagnosis and cross-dataset evaluation** by Qiang Wang, Fang Zhang and San Zhang. Correspondence: taoy.hai.hu@gmail.com.

The data are licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Cite the authors, this record and the version used, and indicate changes. The license applies to the author-generated records and documentation in this repository. Original third-party datasets are not redistributed or relicensed.

## Start here

1. Download the desired ZIP files and the CSV analysis tables, or use **Code > Download ZIP** to obtain the complete repository. Keep the archive names unchanged: the registry uses `Archive.zip::path/inside/archive`.
2. Open `experiment_registry.csv` to locate a run by protocol, experiment family, task, representation and seed. Extract that archive and open its prediction CSV.
3. Each row records the true label and predictions from all five classifiers. Use these rows for downstream analyses; the analysis CSVs provide the corresponding recomputed metrics.
4. Check file integrity using `SHA256SUMS.txt`. `file_manifest.csv` additionally records the original uncompressed prediction and partition file hashes.

## Contents

| File | Contents |
| --- | --- |
| `Main_representation_comparison.zip` | 150 runs, 750 classifier evaluations |
| `CORAL_alignment.zip` | 10 runs, 50 classifier evaluations |
| `Mechanism_ablation.zip` | 150 runs, 750 classifier evaluations |
| `Raw_handcrafted_baseline.zip` | 50 runs, 250 classifier evaluations |
| `Matched_capacity.zip` | 150 runs, 750 classifier evaluations |
| `Partitions_and_metadata.zip` | Training/validation/test partitions, measurement manifests and training-cost records |
| `experiment_registry.csv` | Run inventory and portable paths into the archives |
| `metrics_from_records.csv` | Per-run and per-classifier metrics recomputed from saved predictions |
| `summary_from_records.csv` | Means and sample standard deviations across the five seeds |
| `per_class_from_records.csv`, `confusion_from_records.csv` | Per-class metrics and confusion counts |
| `paper_paired_summary_from_records.csv`, `paired_seed_diagnostics.csv`, `paired_diagnostic_summary.csv` | Matched comparisons, seed-level differences and direction counts |
| `capacity_and_training_cost.csv`, `capacity_training_summary.csv` | Parameter counts, output dimensions, training times and epochs |
| `speed_standardization_diagnostics.csv`, `fusion_rbf_speed_strata.csv`, `constant_prediction_reference.csv`, `ftf_frequency_neighborhood_diagnostics.csv` | Existing-record diagnostics |
| `figure_provenance.csv`, `figure_inputs/` | Figure input snapshots and filtering/aggregation rules |
| `manuscript_figure_files.csv` | Checksums of the separately supplied manuscript figures |
| `data_dictionary.csv`, `repository_metadata.json`, `CITATION.cff` | Field definitions, metadata and citation information |
| `verification.json`, `release_verification.json`, `file_manifest.csv`, `SHA256SUMS.txt` | Metric, inventory and integrity checks |

The 510 prediction CSVs contain **6,004,355 evaluated rows**, with five predictions per row, corresponding to **2,550 classifier evaluations** and 30,021,775 saved predicted labels. Main experiments contribute 750 evaluations, CORAL 50 and the supplements 1,750. Repeated evaluations of a segment across configurations or seeds are not independent physical samples. Random seeds are 42, 43, 44, 45 and 46.

## Experimental scope and labels

Internal CWRU experiments use `Normal`, `IR`, `OR` and `B`; internal MFPT/PU and cross-dataset experiments use the shared `Normal`, `IR` and `OR` classes. `B` denotes rolling element faults and is excluded from cross-dataset closed-set tasks. `IR` and `OR` denote inner and outer race faults. These are direct multiclass predictions, without a fault-detection gate.

Signals were segmented into 2,048-point windows at a stride of 500 points. The CNN representation has 128 dimensions; speed is a common input. Load-related columns preserve measurement metadata and were not input features. Samples were separated by recording or bearing group with class stratification as described in the manuscript. The standard tasks comprise three internal tests, six pairwise cross-dataset tests and CWRU + MFPT training with independent PU testing.

The raw handcrafted baseline sends the standardized TFM plus speed directly to the four traditional classifiers. Its FC classifier is an independent 52→128→64→32→K multilayer perceptron. Capacity controls use a 192-dimensional output and approximately 100,000 trainable parameters; actual counts, training times and epochs are supplied. They are not claims of identical computation or convergence.

## Prediction fields and metric conventions

`TrueLabel` is the actual class. Prediction columns are `FC_Softmax_PredLabel`, `Linear_SVM_PredLabel`, `RBF_SVM_PredLabel`, `Bagged_Trees_PredLabel` and `kNN_PredLabel`. `*_Correct` is 1 for a correct prediction and 0 otherwise. Descriptions expand the class names in English.

`SampleID` identifies a segment and `ExperimentID` identifies a run. `Source_SegmentStart` is a one-based point index; `Source_RPM` is in r/min. Missing provenance cells remain missing and are not inferred. `data_dictionary.csv` defines the prediction and partition fields.

Accuracy is the correct fraction. For each fixed task class, F1 = 2TP/(2TP + FP + FN), with zero when the denominator is zero. Macro-F1 is the arithmetic mean over the fixed task classes. Analysis tables store proportions unless the column explicitly states otherwise; plot percentages multiply proportions by 100. Paired improvements are percentage-point differences, not relative rates. Summary standard deviations use the sample standard deviation over five seeds.

All 2,550 accuracy and Macro-F1 values were independently recomputed from the prediction rows and agreed within 1e-12 absolute tolerance. Test sample/truth digests were consistent across compared configurations within each task and seed. See `verification.json`. The release checks also confirm that the prediction archives are byte-identical to the verified local submission data.

## Reuse boundary and original data

No training source programs, fitted models, raw vibration archives, processed signal tensors or STFT images are included. The shared records support recomputing downstream metrics from saved predictions; they do not reproduce the complete preprocessing and training pipeline. Source code is not publicly provided.

Original dataset routes are the [CWRU Bearing Data Center](https://engineering.case.edu/bearingdatacenter/welcome), [MFPT Fault Data Sets original entry](https://mfpt.org/fault-data-sets/) and [PU Bearing Data Center](https://mb.uni-paderborn.de/kat/forschung/bearing-datacenter/data-sets-and-download). The MFPT package used was `MFPT-Fault-Data-Sets-20200227T131140Z-001 / MFPT Fault Data Sets`. Retained recording mappings are supplied. The original CWRU download IDs cannot all be uniquely recovered from the legacy simplified filenames; this repository does not claim otherwise.

Figures 4–10 are linked to their numerical input snapshots by `figure_provenance.csv`. Figures 1–3 are diagrams. The separate manuscript figure checksums document provenance; figure image files and editable diagram sources are not part of this prediction-data release.

## Citation

Wang, Q., Zhang, F., Zhang, S. (2026). Prediction records for mechanism-based feature fusion in bearing fault diagnosis (Version 1.0.0) [Data set]. GitHub. https://github.com/ForceTaoy/DualPhyNet-prediction-records/tree/v1.0.0

See `CITATION.cff`. The versioned GitHub record has no assigned DOI. Preserve the version and checksums when redistributing or analyzing these records.
