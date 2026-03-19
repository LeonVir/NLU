---
{}
---
language: en
license: cc-by-4.0
tags:
- text-classification
- authorship-verification
- roberta-large
- asymmetric-loss
- pytorch
- repo: https://github.com/LeonVir/NLU

---

# Model Card for H18609dp-W29498jk-AV

<!-- Provide a quick summary of what the model is/does. -->

A Deep Learning model for Authorship Verification based on RoBERTa-Large.
Given two text sequences, it determines whether they were written by the same author. 
It utilizes Asymmetric Loss (ASL) with a 2:1 focal parameter ratio to strongly focus on hard-to-classify samples and mitigate over-confidence issues, achieving a highly balanced Macro F1-score.


## Model Details

### Model Description

<!-- Provide a longer summary of what this model is. -->

This model is a fine-tuned version of the pre-trained RoBERTa-Large encoder, utilizing its standard sequence classification head. 

Data Preprocessing: Before tokenization, the raw text underwent a targeted cleaning process using regular expressions. We explicitly removed email headers (e.g., From, To, Cc, Subject), individual email addresses, and URLs. Finally, all redundant whitespaces were normalized. This ensures the RoBERTa model focuses entirely on the author's intrinsic linguistic style rather than structural metadata or links. 

Input Representation: Processed text pairs are concatenated with RoBERTa's standard separation tokens (`<s> text1 </s></s> text2 </s>`) and fed into the model (up to a maximum sequence length of 512 tokens).

Asymmetric Loss (ASL) Integration: The key innovation of this model is the implementation of Asymmetric Loss instead of standard Cross-Entropy. We explicitly configured the asymmetric focusing parameters with a 2:1 ratio (gamma_neg = 2, gamma_pos = 1). This asymmetric setup dynamically down-weights the loss from easily classified examples much more aggressively. Consequently, it forces the deep RoBERTa-Large network to heavily penalize and learn from hard-negative text pairs with subtle stylistic differences. This targeted optimization successfully resolved the initial decision threshold bias, shifting the optimal threshold to a highly balanced 0.51 and significantly boosting overall predictive performance.

- **Developed by:** Dongmyung Park & Juho Kim
- **Language(s):** English
- **Model type:** Supervised Fine-tuning
- **Model architecture:** Transformers (RoBERTa-Large + Standard Sequence Classification Head)
- **Finetuned from model [optional]:** roberta-large

### Model Resources

<!-- Provide links where applicable. -->

- **Repository:** https://huggingface.co/roberta-large
- **Paper or documentation:** https://arxiv.org/abs/1907.11692

## Training Details

### Training Data

<!-- This is a short stub of information on the training data that was used, and documentation related to data pre-processing or additional filtering (if applicable). -->

27K+ sequence pairs from the COMP34812 Authorship Verification (AV) track dataset.
Text pairs are drawn from various domains. No external datasets were used (closed track).
Preprocessing is strictly limited to the removal of email headers, email addresses, URLs, and whitespace normalization to preserve the core stylometric features of the text.

### Training Procedure

<!-- This relates heavily to the Technical Specifications. Content here should link to that section when it is relevant to the training procedure. -->

#### Training Hyperparameters

<!-- This is a summary of the values of hyperparameters used in training the model. -->

- learning_rate: 5e-06
- train_batch_size: 32
- eval_batch_size: 32
- seed: 10879360
- num_epochs: 14 (Early stopping triggered at epoch 13)
- warmup_ratio: 0.1
- weight_decay: 0.01
- loss_function: Asymmetric Loss (gamma_neg=2, gamma_pos=1)
- decision_threshold: 0.51

#### Speeds, Sizes, Times

<!-- This section provides information about how roughly how long it takes to train the model and the size of the resulting model. -->

- Overall Training Time: ~6 hours 11 minutes (Accelerated via 1x NVIDIA A100-SXM4-40GB)
- Duration per Training Epoch: ~28.5 minutes
- Model Weights: ~1.32 GB (model.safetensors: 1,388,180 KB)
- Tokenizer: ~3.4 MB (tokenizer.json: 3,476 KB, tokenizer_config.json: 1 KB)
- Configuration & Inference: ~2 KB (config.json: 1 KB, best_threshold.json: 1 KB)

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

- F1-score (Macro avg): 0.8348
- F1-score (Binary - Class 1): 0.8365
- Accuracy: 0.83
- Precision (Macro avg): 0.83
- Recall (Macro avg): 0.83

Detailed Performance by Class:
- Class 0 (Different Author): Precision: 0.83, Recall: 0.84, F1-score: 0.83
- Class 1 (Same Author): Precision: 0.84, Recall: 0.83, F1-score: 0.84

## Technical Specifications

### Hardware

Training:
- CPU: Multi-core CPU (Required for data loading, tokenization, and managing PyTorch dataloaders)
- System RAM: 32 GB minimum (To safely load the dataset and handle deep learning framework overhead)
- Storage: ~5 GB (Sufficient for the dataset, Python environment, and saving 1.4 GB model checkpoints)
- GPU: 1x NVIDIA A100-SXM4-40GB (Required for accelerating the heavy computational cost of 355M parameters)

Inference:
- CPU: Standard CPU (Manages the inference pipeline and tokenization)
- System RAM: 16 GB minimum (Required to load the large 1.32 GB model weights into memory before VRAM transfer)
- Storage: ~2 GB (To store the model weights, tokenizer files, and configuration)
- GPU: Highly recommended for practical latency. Pure CPU inference is possible but not recommended due to extreme slowness.

### Software

- Python 3.10+
- PyTorch == 2.10.0+cu128 (Compiled with CUDA 12.8 for A100 acceleration)
- Transformers == 5.2.0
- Datasets (Hugging Face)
- scikit-learn == 1.6.1
- pandas == 2.3.3
- numpy == 2.0.2
- Jinja2 == 3.1.6

## Bias, Risks, and Limitations

<!-- This section is meant to convey both technical and sociotechnical limitations. -->

- Input Truncation: RoBERTa-Large has a strict maximum sequence length of 512 tokens. Any text pairs (concatenated) exceeding this limit are truncated, potentially losing critical stylometric clues located at the end of longer texts.
- Language Bias: The model is pre-trained exclusively on English corpora. Performance on multilingual, code-switched, or heavily dialectal text is not guaranteed.
- Computational Cost: Unlike traditional machine learning ensembles (Category A), this large-scale transformer requires significant GPU memory and compute power, making it challenging to deploy on edge devices.

## Additional Information

<!-- Any other information that would be useful for other people to know. -->

- Threshold Optimization: After training, the decision threshold was empirically optimized. Scanning probability thresholds between 0.2 and 0.8 revealed that 0.51 maximized the Macro F1-score on the dev set.
- Loss Function Dynamics: The ASL hyperparameters (gamma_neg=2, gamma_pos=1) were specifically targeted to heavily penalize hard-to-classify 'different-author' pairs without destabilizing the learning of 'same-author' pairs.
- Task Category: This model is submitted under Category C (Deep Learning & Transformers) for the COMP34812 Authorship Verification task.
- Model Storage: The .safetensors model weights and tokenization artifacts are securely stored on [Google Drive](Link).
