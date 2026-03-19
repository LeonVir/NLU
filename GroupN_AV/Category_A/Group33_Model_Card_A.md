---
{}
---
language: en
license: cc-by-4.0
tags:
- text-classification
- authorship-verification
- xgboost
- lightgbm
- stylometry
- repo: https://github.com/LeonVir/NLU

---

# Model Card for H18609dp-W29498jk-AV

<!-- Provide a quick summary of what the model is/does. -->

A traditional machine learning ensemble model for Authorship Verification.
Given two text sequences, the model determines whether they were written by the same author. 
It combines XGBoost and LightGBM classifiers using TF-IDF and hand-crafted stylometric features.


## Model Details

### Model Description

<!-- Provide a longer summary of what this model is. -->

This model is a supervised ensemble of **XGBoost** and **LightGBM** classifiers.

Input text pairs are represented using:
- Word-level TF-IDF (1-2 grams)
- Character-level TF-IDF (2-4 grams)
- Function Word TF-IDF: Uses a predefined vocabulary of common English function words (1-2 grams) to capture subconscious stylistic habits.
- 24 hand-crafted stylometric features (e.g., word length, sentence length, punctuation ratios, lexical diversity). 

Pairwise feature interactions are captured via absolute difference, element-wise product, and cosine similarity. The two models are finally combined via a weighted probability ensemble with an optimised decision threshold.

- **Developed by:** Dongmyung Park & Juho Kim
- **Language(s):** English
- **Model type:** Supervised
- **Model architecture:** Traditional Machine Learning (XGBoost + LightGBM Ensemble)
- **Finetuned from model [optional]:** N/A (trained from scratch)

### Model Resources

<!-- Provide links where applicable. -->

- **Repository:** N/A
- **Paper or documentation:** N/A

## Training Details

### Training Data

<!-- This is a short stub of information on the training data that was used, and documentation related to data pre-processing or additional filtering (if applicable). -->

27K+ sequence pairs from the COMP34812 Authorship Verification (AV) track dataset.
Text pairs are drawn from various domains. No external datasets were used (closed track).
Preprocessing includes removal of emails, URLs, dates, phone numbers,
repeated characters, and whitespace normalisation.

### Training Procedure

<!-- This relates heavily to the Technical Specifications. Content here should link to that section when it is relevant to the training procedure. -->

#### Training Hyperparameters

<!-- This is a summary of the values of hyperparameters used in training the model. -->


XGBoost:
- n_estimators: 7000 (Early stopping triggered at iteration 6213)
- learning_rate: 0.01
- max_depth: 7
- subsample: 0.8
- colsample_bytree: 0.8
- min_child_weight: 2
- max_bin: 128
- early_stopping_rounds: 200
- random_state: 10879360

LightGBM:
- n_estimators: 2000 (Early stopping triggered at iteration 1700)
- learning_rate: 0.01
- num_leaves: 127
- subsample: 0.8
- subsample_freq: 1
- colsample_bytree: 0.8
- max_bin: 127
- early_stopping_rounds: 100
- random_state: 10879360

Ensemble:
- XGB weight: 0.26
- LGB weight: 0.74
- Decision threshold: 0.43

#### Speeds, Sizes, Times

<!-- This section provides information about how roughly how long it takes to train the model and the size of the resulting model. -->


- Total training time: 1h 10m 1s
- XGBoost Training: 27m 17s (Total 6,213 iterations, ~26.7s per 100 rounds)
- LightGBM Training: 59m 19s (Total 1,700 iterations, ~222.4s per 100 rounds)
- XGBoost model size: ~29.87 MB (30,586 KB)
- LightGBM model size: ~24.18 MB (24,759 KB)
- TF-IDF vectorizers: 437 KB total (Word: 229 KB, Char: 203 KB, Function Word: 5 KB)
- Ensemble config: 1 KB

## Evaluation

<!-- This section describes the evaluation protocols and provides the results. -->

### Testing Data & Metrics

#### Testing Data

<!-- This should describe any evaluation data used (e.g., the development/validation set provided). -->

The model was evaluated using the full development set provided for the COMP34812 Authorship Verification (AV) track. 

- Total Samples: 5,993 sequence pairs.
- Label Distribution: 
    - Class 0 (Different Author): 2,937 pairs (49.01%)
    - Class 1 (Same Author): 3,056 pairs (50.99%)
- Data Characteristics: The evaluation set is well-balanced, ensuring that metrics like Accuracy and Macro F1-score provide a reliable assessment of the model's discriminative power across both classes.
- Independence: This dataset was strictly separated from the training set to ensure the evaluation of the model's generalization capabilities on unseen text pairs.

#### Metrics

<!-- These are the evaluation metrics being used. -->

- F1-score
- Accuracy
- Precision
- Recall

### Results

- F1-score (Macro avg): 0.7702
- F1-score (Binary - Class 1): 0.7813
- Accuracy: 0.77
- Precision (Macro avg): 0.77
- Recall (Macro avg): 0.77

Detailed Performance by Class:
- Class 0 (Different Author): Precision: 0.78, Recall: 0.74, F1-score: 0.76
- Class 1 (Same Author): Precision: 0.76, Recall: 0.80, F1-score: 0.78

## Technical Specifications

### Hardware

Training:
- CPU: Multi-core CPU (Required for data preprocessing, TF-IDF vectorization, and managing parallel training threads)
- System RAM: 16 GB minimum (To handle high-dimensional sparse matrices during TF-IDF fitting)
- Storage: ~2 GB (Sufficient for the COMP34812 dataset, Python environment, and saving model artifacts)
- GPU: 2x NVIDIA T4 (16 GB VRAM per GPU) utilized for parallelized boosting
Inference:
- CPU: Standard CPU (Sufficient for real-time, millisecond-level latency)
- System RAM: 2 GB minimum (Extremely lightweight for deployment)
- Storage: < 100 MB (The combined size of the ensemble models and TF-IDF vectorizers is under 60 MB)
- GPU: Not required

### Software

- Python 3.10+
- scikit-learn == 1.6.1
- xgboost == 3.1.3
- lightgbm == 4.6.0
- pandas == 2.3.3
- numpy == 2.0.2
- scipy == 1.16.3
- nltk == 3.9.1
- joblib == 1.5.3
- Jinja2 == 3.1.6

## Bias, Risks, and Limitations

<!-- This section is meant to convey both technical and sociotechnical limitations. -->

- Language Constraint: The model is strictly optimized for English text. Performance on multilingual or code-switched text is not guaranteed.
- Input Length Sensitivity: Stylometric feature extraction (24-feature set) requires a minimum of 5 words. Pairs with extremely short texts may result in degraded predictive accuracy.
- Out-of-Vocabulary (OOV) Issue: Since TF-IDF vectorizers are fitted on the training corpus, stylistic markers or unique tokens not present during training are ignored.
- Domain Bias: Stylistic patterns may vary across genres (e.g., formal news vs. informal blog posts). The model may reflect the specific domain biases of the COMP34812 training corpus.
- Ethical Considerations: Authorship verification technology should be used responsibly. Potential risks include the de-anonymization of anonymous authors without consent.

## Additional Information

<!-- Any other information that would be useful for other people to know. -->

- Efficiency: XGBoost and LightGBM were trained in parallel using Python's threading module to maximize GPU (2x NVIDIA T4) utilization.
- Optimization: Final ensemble weights (0.26 for XGB, 0.74 for LGBM) and the decision threshold (0.43) were determined via extensive grid search on the development set to maximize the Macro F1-score.
- Task Category: This model belongs to Category A (Traditional ML & Stylometry) of the COMP34812 Authorship Verification task.
- Model Storage: Trained artifacts (serialized .json and .pkl files) and vectorizers are stored on [Google Drive](Link).
