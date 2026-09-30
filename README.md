# Rubric-Guided LLM Annotation for Sentiment Analysis: Holistic Labels, Aspects, and Evidence

Code for the manuscript of the same title. A rubric-guided gpt-4o-mini annotation of 102,061 patient reviews from the UCI Drug Review dataset records a holistic sentiment label, a class-score vector, a state for each of four aspects (efficacy, safety, burden, cost), and the passages supporting each aspect state. Seven supervision designs ("rungs") learn from this annotation to answer three research questions. The repository follows the structure of the manuscript.

## Layout

| Folder | Manuscript | Contents |
|---|---|---|
| [`data/`](data/) | Section 3.1 | Narrative-group split, with the 500 human-verification reviews held out before any model was trained |
| [`analysis/`](analysis/) | Section 4 | Notebooks that read the annotation and the saved model outputs and produce every reported table |
| [`RQ1/`](RQ1/) | Section 4.2.1 | Learning the holistic label from the text, compared with the writer's rating (rungs 1–2) |
| [`RQ2/`](RQ2/) | Section 4.2.2 | Connecting the aspects and the holistic label (rungs 3–6) |
| [`RQ3/`](RQ3/) | Section 4.2.3 | Supervising the textual evidence (rung 7) |

### Model notebooks by research question

Each folder holds one training notebook. Rungs 1–2 fit seven model families (logistic regression, random forest, LightGBM, GRU, CNN, ALBERT, BioBERT); rungs 3–7 fit ALBERT and BioBERT. Folders ending in `_metadata` train the same design with the review's condition and ingredients as additional input.

| Folder | Rung and configuration (manuscript name) | Notebook |
|---|---|---|
| `RQ1/rung1_rating` | Rung 1: binned writer rating | `DrugReview_OriginalLabel_multiclass.ipynb` |
| `RQ1/rung1_rating_metadata` | Rung 1 with metadata | `DrugReview_OriginalLabel_multiclass.ipynb` |
| `RQ1/rung2_holistic` | Rung 2: holistic label | `DrugReview_4ominiLabel_multiclass.ipynb` |
| `RQ1/rung2_holistic_metadata` | Rung 2 with metadata | `DrugReview_4ominiLabel_multiclass.ipynb` |
| `RQ2/rung3_aspects` | Rung 3: aspect baseline | `DrugReview_auxiliary_supervision.ipynb` |
| `RQ2/rung3_aspects_metadata` | Rung 3 with metadata | `DrugReview_auxiliary_supervision.ipynb` |
| `RQ2/rung4_parallel` | Rung 4: parallel, with aspect heads | `DrugReview_auxiliary_supervision.ipynb` |
| `RQ2/rung4_parallel_metadata` | Rung 4 with aspect heads, with metadata | `DrugReview_auxiliary_supervision.ipynb` |
| `RQ2/rung4_no_aspect_heads` | Rung 4: parallel, without aspect heads | `DrugReview_matched_holistic_control.ipynb` |
| `RQ2/rung5_nested` | Rung 5: fusion | `DrugReview_confirmatory_representation_only.ipynb` |
| | Rung 5: residual | `DrugReview_confirmatory_representation_residual.ipynb` |
| | Rung 5: joint bottleneck | `DrugReview_confirmatory_probability_bottleneck.ipynb` |
| | Rung 5: detached bottleneck | `DrugReview_confirmatory_detached_bottleneck.ipynb` |
| `RQ2/rung6_anchored_free` | Rung 6: 4 anchored + 4 free | `DrugReview_semi_confirmatory_4anchored_4free.ipynb` |
| | Rung 6: 4 anchored + 0 free | `DrugReview_semi_confirmatory_4anchored_0free.ipynb` |
| | Rung 6: 0 anchored + 4 free | `DrugReview_semi_confirmatory_0anchored_4free.ipynb` |
| `RQ3/rung7_evidence` | Rung 7: evidence loss on (η = 1) | `DrugReview_evidence_hierarchical_evidence_1.ipynb` |
| | Rung 7: evidence loss off (η = 0) | `DrugReview_evidence_hierarchical_evidence_0.ipynb` |

### Where each table comes from

The analysis notebooks fit no encoder: they score the saved predictions of the selected models, and only the fixed readouts of rung-3 probabilities are fitted there. Each notebook opens with a table of its sections and the manuscript results they reproduce.

| Manuscript | Notebook (section) |
|---|---|
| Table 2; Supplementary Table S2 | [`analysis/DrugReview_Annotation_Summary.ipynb`](analysis/DrugReview_Annotation_Summary.ipynb) |
| Section 3.3: share of reviews within the 200-token window | [`analysis/DrugReview_Annotation_Summary.ipynb`](analysis/DrugReview_Annotation_Summary.ipynb) (4) |
| Table 3 | [`analysis/DrugReview_Entropy_Analysis.ipynb`](analysis/DrugReview_Entropy_Analysis.ipynb) (10) |
| Table 4; Supplementary Table S5 | [`analysis/Human_Validation_Model_Agreement.ipynb`](analysis/Human_Validation_Model_Agreement.ipynb) (1, 4) |
| Table 5; Supplementary Table S6 | [`analysis/DrugReview_Ladder_Evaluation.ipynb`](analysis/DrugReview_Ladder_Evaluation.ipynb) (1) |
| Table 6; Supplementary Tables S7 and S8 | [`analysis/Human_Validation_Model_Agreement.ipynb`](analysis/Human_Validation_Model_Agreement.ipynb) (2) |
| Tables 7–10; Supplementary Tables S9, S10, and S12 | [`analysis/DrugReview_Ladder_Evaluation.ipynb`](analysis/DrugReview_Ladder_Evaluation.ipynb) (2–7) |
| Tables 11–12; Supplementary Tables S11, S13, and S14 | [`analysis/DrugReview_Readouts_Factors_Localization.ipynb`](analysis/DrugReview_Readouts_Factors_Localization.ipynb) (1–5) |
| Supplementary Table S15 | [`analysis/Human_Validation_Model_Agreement.ipynb`](analysis/Human_Validation_Model_Agreement.ipynb) (3) |

[`analysis/Human_Validation_Agreement_Analysis.ipynb`](analysis/Human_Validation_Agreement_Analysis.ipynb) is the companion field-by-field report of the two annotators and the annotation. The notebooks that produce the tables keep the outputs of their runs, so every reported number can be checked against the manuscript.

## Data root

The notebooks do not read from the folders of this repository. They read their inputs and write their outputs under a data root that keeps the layout of the original project, whose folder names differ from the repository's:

| Repository folder | Data-root folder |
|---|---|
| `data/split` | `00_split` |
| `RQ1/rung1_rating`, `RQ1/rung1_rating_metadata` | `01_OriginalLabel`, `01_OriginalLabel_aug` |
| `RQ1/rung2_holistic`, `RQ1/rung2_holistic_metadata` | `02_4ominiLabel`, `02_4ominiLabel_aug` |
| `RQ2/rung3_aspects`, `RQ2/rung3_aspects_metadata` | `03_Full_aspects`, `03_Full_aspects_aug` |
| `RQ2/rung4_parallel`, `RQ2/rung4_parallel_metadata` | `04_Holistic_all_aspects`, `04_Holistic_all_aspects_aug` |
| `RQ2/rung4_no_aspect_heads` | `ablation/A0_Matched_holistic_control` |
| `RQ2/rung5_nested` | `05_Confirmatory` |
| `RQ2/rung6_anchored_free` | `06_Semi_confirmatory` |
| `RQ3/rung7_evidence` | `07_Evidence_hierarchical` |
| `analysis` (outputs) | `00_annotation_summary/outputs`, `00_entropy/outputs`, `00_human_validation/outputs/model_agreement`, `08_Manuscript_evaluation/outputs` |

The analysis notebooks also read the annotation (`00_labelling/output_v2/v2_ef356fdf87/`, including the class-score stream `s_7c599892d6`), the human-verification sample key and the two annotators' workbooks (`00_human_validation/`), and the split manifest (`00_split/data/DrugReview_split.csv`).

## Running the notebooks

The notebooks were run in Google Colab with the data root on Google Drive. For the split and analysis notebooks, set `DRUGREVIEW_ROOT` to the data root when it is not found automatically; the model notebooks set their project path in their first cells. Classical models and the analyses run on CPUs; the GRU, CNN, and transformers use a GPU. The recorded environment is Python 3.13.15, scikit-learn 1.6.1, PyTorch 2.11.0, and Transformers 4.57.6.

This repository contains code only. The review text comes from the UCI Drug Review dataset, released through the [UCI Machine Learning Repository](https://web.archive.org/web/20241125/https://archive.ics.uci.edu/dataset/462) and also publicly available on [Kaggle](https://www.kaggle.com/datasets/jessicali9530/kuc-hackathon-winter-2018); its terms of use do not permit redistribution. The annotation, the annotators' labels, the saved model artifacts, and the label-generation notebooks are not part of this repository.
