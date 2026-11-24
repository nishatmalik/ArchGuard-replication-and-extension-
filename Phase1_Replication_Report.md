
# Improving LLM-Based Early Warning of Architectural Decay Using Prompt Engineering and Retrieval: A Replication and Extension of ArchGuard

## Phase 1: Replication Report

### Executive Summary

This study investigates early warning of architectural decay
through the prediction of architectural smell-inducing (ASI)
software issues.

The replication reconstructs the classification datasets and
reproduces the supervised SVM baseline using the released
OpenAI embeddings and detector-specific ground-truth labels.

The SVM results match the previously recorded published
F1-score references at three-decimal reporting precision.

The released GPT-5-mini prediction cache exhibits zero
positive-class recall and F1-score. This result characterizes
the archived predictions and does not establish the
performance of fresh GPT-5-mini inference.

## 1. Dataset Characteristics

The classification dataset contains 6,390
detector-specific records corresponding to 4,463
unique issues across three software projects.

There are 940 ASI and 5,450
non-ASI records. The overall ASI prevalence is
14.71%.

| project   | detector   |   total_records |   ASI |   non_ASI |   ASI_percentage |
|:----------|:-----------|----------------:|------:|----------:|-----------------:|
| inkscape  | arcade     |            2536 |   258 |      2278 |          10.1735 |
| shepard   | arcade     |             346 |    46 |       300 |          13.2948 |
| shepard   | arcan      |             346 |   120 |       226 |          34.6821 |
| stackgres | arcade     |            1581 |   279 |      1302 |          17.6471 |
| stackgres | arcan      |            1581 |   237 |      1344 |          14.9905 |

## 2. Experimental Methodology

- Five project-detector configurations.
- Two repetitions of five-fold stratified cross-validation.
- Fifty SVM fold evaluations in total.
- Released text-embedding-3-small embeddings.
- RBF support vector machine.
- Random undersampling restricted to training folds.
- Positive-class precision, recall and F1-score.
- Matched-record analysis for archived LLM comparisons.

The original embedding input identity and exact original fold
identity remain independently unverified.

## 3. SVM Replication Results

| detector   |   precision |   recall |     f1 |   published_reference_f1 |   absolute_f1_difference | matches_reporting_precision   |
|:-----------|------------:|---------:|-------:|-------------------------:|-------------------------:|:------------------------------|
| arcade     |      0.2428 |   0.7098 | 0.36   |                    0.36  |                   0      | True                          |
| arcan      |      0.4048 |   0.7412 | 0.5058 |                    0.506 |                   0.0002 | True                          |

The numerical reproduction matches the recorded published
F1-score references at three decimal places.

## 4. Matched Baseline Evaluation

| project   | detector   | cohort         |   accuracy_svm |   accuracy_llm |   f1_svm |   f1_llm |   recall_svm |   recall_llm |
|:----------|:-----------|:---------------|---------------:|---------------:|---------:|---------:|-------------:|-------------:|
| inkscape  | arcade     | cached_matched |         0.7063 |         0.888  |   0.3159 |        0 |       0.6013 |            0 |
| shepard   | arcade     | cached_matched |         0.6703 |         0.8643 |   0.3754 |        0 |       0.7256 |            0 |
| shepard   | arcan      | cached_matched |         0.6813 |         0.6489 |   0.6138 |        0 |       0.7145 |            0 |
| stackgres | arcade     | cached_matched |         0.6022 |         0.8229 |   0.4173 |        0 |       0.8029 |            0 |
| stackgres | arcan      | cached_matched |         0.6553 |         0.8483 |   0.4015 |        0 |       0.7658 |            0 |
| inkscape  | arcade     | valid_matched  |         0.7085 |         0.891  |   0.3072 |        0 |       0.5913 |            0 |
| shepard   | arcade     | valid_matched  |         0.6987 |         0.9022 |   0.2986 |        0 |       0.6529 |            0 |
| shepard   | arcan      | valid_matched  |         0.7283 |         0.6826 |   0.6472 |        0 |       0.7701 |            0 |
| stackgres | arcade     | valid_matched  |         0.6135 |         0.8307 |   0.4103 |        0 |       0.7938 |            0 |
| stackgres | arcan      | valid_matched  |         0.6711 |         0.8588 |   0.3863 |        0 |       0.7374 |            0 |

## 5. Archived LLM Response Reliability

| Metric | Value |
|---|---:|
| Cached predictions | 6,042 |
| Version-excluded records | 348 |
| Valid responses | 5,426 |
| Invalid responses | 616 |
| Empty responses | 606 |
| Valid response rate | 89.80% |
| End-to-end coverage | 84.91% |
| Actual ASI among invalid responses | 157 |
| True positives | 0 |
| False positives | 2 |
| False negatives | 929 |
| True negatives | 5111 |

All missing archived predictions were explained by the
original version-exclusion criterion.

Invalid responses were assigned negative labels in the
released cache. Strict parsing did not change the labels
of responses classified as valid by the audit.

The cause of empty responses has not been established.

## 6. Class-Specific Response Failure

| project   | detector   |   ASI_invalid_rate |   non_ASI_invalid_rate |   failure_risk_ratio |
|:----------|:-----------|-------------------:|-----------------------:|---------------------:|
| inkscape  | arcade     |             0.0927 |                 0.0646 |               1.4357 |
| shepard   | arcade     |             0.4565 |                 0.2184 |               2.09   |
| shepard   | arcan      |             0.3277 |                 0.2182 |               1.5021 |
| stackgres | arcade     |             0.1219 |                 0.0725 |               1.6802 |
| stackgres | arcan      |             0.1688 |                 0.0942 |               1.7922 |

The overall observed ASI response failure rate was
16.90%, compared
with 8.98%
for non-ASI records.

The descriptive failure risk ratio was
1.8826.

This is an observational association, not a causal result.
Records shared between detectors are not independent.

## 7. Error Analysis

- ASI detected in both SVM runs:
  612
- ASI missed in both SVM runs:
  192
- Non-ASI misclassified in both runs:
  1442
- Configurations showing the accuracy paradox:
  4 of 5.

## 8. Detector Label Agreement

| project   |   shared_issues |   agreements |   disagreements |   agreement_rate |
|:----------|----------------:|-------------:|----------------:|-----------------:|
| shepard   |             346 |          260 |              86 |           0.7514 |
| stackgres |            1581 |         1249 |             332 |           0.79   |

The two detectors disagree on
418 shared
issues. Detector-derived labels should therefore be treated
as operational ground-truth proxies.

## 9. Research Findings

| finding_id   | finding                                                   | status                  |
|:-------------|:----------------------------------------------------------|:------------------------|
| F01          | SVM reproduction at published precision                   | confirmed               |
| F02          | Released GPT-5-mini cache has zero ASI recall             | confirmed               |
| F03          | Missing predictions explained by version filter           | confirmed               |
| F04          | Invalid responses silently classified as negative         | confirmed               |
| F05          | Strict parsing alone does not improve cached F1           | confirmed               |
| F06          | Invalid responses occur more frequently among ASI records | descriptively_supported |
| F07          | Output token exhaustion explains empty responses          | unverified              |
| F08          | Prompt engineering improves ASI prediction                | pending_phase_2         |
| F09          | Retrieval augmentation improves ASI prediction            | pending_phase_2         |

## 10. Threats to Validity

| id   | limitation                                                            |
|:-----|:----------------------------------------------------------------------|
| L01  | No fresh LLM inference performed.                                     |
| L02  | Published LLM versus cache discrepancy unresolved.                    |
| L03  | Original embedding input identity not fully verified.                 |
| L04  | Exact original fold identity not independently verified.              |
| L05  | Repeated cross-validation folds are dependent.                        |
| L06  | Detector-specific records can represent the same issue.               |
| L07  | Smell detector labels are imperfect ground-truth proxies.             |
| L08  | Random cross-validation does not demonstrate temporal generalization. |
| L09  | Published LLM metric transcription requires source re-verification.   |

The strict response parser is a heuristic validity check,
not independent semantic verification.

No fresh LLM inference was performed during this phase.

## 11. Proposed Research Extension

The proposed investigation evaluates whether reliability
controls, prompt engineering, retrieval-augmented few-shot
classification and structured architectural reasoning improve
early ASI prediction.

The experiments must isolate the effects of these
interventions using comparable held-out records.

Retrieved demonstrations must originate exclusively from
the permitted training partition.

Prompt development must not use held-out test labels.
Because Phase 1 involved exploratory analysis of the
existing dataset, independent or temporal confirmation is
desirable for a stronger final generalization claim.

## 12. Proposed Experimental Conditions

| condition   | method                             | example_selection   | purpose                           | experimental_status   |
|:------------|:-----------------------------------|:--------------------|:----------------------------------|:----------------------|
| E0          | Archived zero-shot                 | none                | Existing replication artifact     | completed             |
| E1          | Fresh reliable zero-shot           | none                | Fresh reference baseline          | planned               |
| E2          | Enhanced zero-shot prompt          | none                | Prompt engineering effect         | planned               |
| E3          | Random few-shot                    | random              | Random example selection control  | planned               |
| E4          | Retrieval-augmented few-shot       | similarity          | Similarity-based retrieval effect | planned               |
| E5          | Structured architectural reasoning | none                | Structured reasoning effect       | planned               |
| E6          | Proposed hybrid approach           | similarity          | Combined intervention             | planned               |

## 13. Hypothetical Performance Targets

| detector   |   svm_reference_precision |   svm_reference_recall |   svm_reference_f1 |   svm_reference_balanced_accuracy |   svm_reference_mcc |   cached_llm_f1 |   target_f1_5pct_relative_gain |   target_f1_10pct_relative_gain |   target_f1_20pct_relative_gain |
|:-----------|--------------------------:|-----------------------:|-------------------:|----------------------------------:|--------------------:|----------------:|-------------------------------:|--------------------------------:|--------------------------------:|
| arcade     |                    0.2428 |                 0.7098 |             0.36   |                            0.6774 |              0.2524 |               0 |                         0.378  |                          0.396  |                           0.432 |
| arcan      |                    0.4048 |                 0.7412 |             0.5058 |                            0.6951 |              0.3283 |               0 |                         0.5311 |                          0.5564 |                           0.607 |

These targets are planning benchmarks and are not
experimental achievements.

## 14. Scientific Figures

| figure                            | title                                                   |
|:----------------------------------|:--------------------------------------------------------|
| fig01_dataset_distribution        | Dataset class distribution                              |
| fig02_asi_prevalence              | Architectural smell-inducing record prevalence          |
| fig03_matched_baseline_comparison | Matched SVM versus cached GPT-5-mini                    |
| fig04_svm_performance             | SVM precision, recall and F1-score                      |
| fig05_accuracy_paradox            | Accuracy paradox                                        |
| fig06_response_reliability        | End-to-end LLM response reliability                     |
| fig07_class_specific_failure      | Class-specific LLM response failure                     |
| fig08_confusion_matrices          | Matched confusion matrices using one CV run             |
| fig09_cv_distribution             | Descriptive cross-validation F1 distributions           |
| fig10_error_stability             | ASI error stability across CV runs                      |
| fig11_text_length_analysis        | Text length, classification error and response coverage |
| fig12_detector_agreement          | Cross-detector label agreement                          |
| fig13_published_vs_reproduced     | Published versus reproduced SVM performance             |
| fig14_phase2_targets              | Hypothetical Phase 2 performance targets                |

## 15. Conclusion

Phase 1 establishes a numerically reproduced SVM baseline
and documents limitations in the released LLM predictions.

The archived response failure patterns motivate a controlled
investigation of reliable LLM inference and enhanced
classification methods.

Whether these methods improve ASI prediction remains an
open experimental question.

## Replication Artifact

https://zenodo.org/doi/10.5281/zenodo.18891082

The original publication's LLM metric transcription and
the discrepancy with the released prediction cache remain
subject to source verification.
