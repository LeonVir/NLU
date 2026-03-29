# Model Execution & Reproducibility

## 1. Group Information
### 👥 Team Composition
* **Group Number:** Group 33
* **Team Members:**
  1. **Dongmyung Park** (Student ID: 10879360)
  2. **Juho Kim** (Student ID: 10466626)

---

## 2. Directory Structure & Inventory
To ensure a seamless grading process, we have structured our submission as follows. Please unzip the **Group_33_AV.zip** and maintain the internal hierarchy to prevent file path exceptions.

```text
📁 Group_33_AV.zip
│
├── 📄 README.md                           # Execution guide & Critical external links (OneDrive/G-Drive)
├── 📄 requirements.txt                    # Environment dependencies
│
├── 📁 Category_A/                         # Machine Learning Task (XGBoost/LightGBM)
│   ├── 💻 Group_33_Training_A.ipynb       # Full model training & Hyperparameter tuning
│   ├── 💻 Group_33_Demo_A.ipynb           # Model loading & Final inference pipeline
│   ├── 📊 Group_33_ModelCard_A.md         # Modelcard for model A
│   └── 📝 Group_33_A.csv                  # Prediction csv for model A
│
└── 📁 Category_C/                         # Deep Learning Task (Transformers/RoBERTa)
    ├── 💻 Group_33_Training_C_ASL.ipynb   # Training with ASL (Asymmetric Loss)
    ├── 💻 Group_33_Demo_C.ipynb           # Model loading & GPU-accelerated inference
    ├── 📊 Group_33_ModelCard_C.md         # Modelcard for model C
    └── 📝 Group_33_C.csv                  # Prediction csv for model C
```

---

## 3. Model Weights Download Links (Action Required)
Due to the Canvas submission file size limit (10MB), our pre-trained model weights (total approx. 1.4GB) are hosted on Google Drive. 

**IMPORTANT:** To ensure the notebooks run correctly without path errors, please download the files from the links below and place them exactly in their respective sub-directories as specified.

* **Category A Model (XGBoost/LightGBM)**
    * **Download:** [Category A Google Drive Link](https://drive.google.com/drive/folders/1mF0bMpzfs3XODpQA2Hd7H_JvXLk5AGgp?usp=sharing)
    * **Target Path:** 📁 Category_A
    * **✅️ Action: Download the folder and place it directly inside the 'Category_A' directory.**

* **Category C Model (RoBERTa)**
    * **Download:** [Category C Google Drive Link](https://drive.google.com/drive/folders/1Ty9-EpOLAgzvTWPuqrHBTbehmZZuDs3V?usp=sharing)
    * **Target Path:** 📁 Category_C
    * **✅️ Action: Download the folder and place it directly inside the 'Category_C' directory.**

**Note:** Link sharing is set to **"Anyone with the link can view/download"**. If you encounter any access issues, please contact the team members immediately.

---

## 4. Environment Setup & Execution Guide
This project was developed and validated on **Python 3.12.7** (Windows/Anaconda). To ensure full reproducibility and GPU acceleration, please follow this execution flow carefully.

### Step 1: Environment Check
Please review our **requirements.txt**. This file specifies the environment for running the **demo (inference) notebooks**. Note that 
the training environments differ per solution — exact versions used during training are documented in each model card. 
The most critical requirement for this project is **PyTorch with CUDA 12.1 support** (torch==2.5.1+cu121). 
* If your current environment already has the required libraries installed, you may **skip the installation** and proceed to Step 3.

**⚠️ Disclaimer:** If you choose to run the notebooks without installing our requirements and encounter any runtime errors, we strongly request that you configure your environment to match our exact setup (Python 3.12.7 + CUDA 12.1 PyTorch) and try again.

---

### Step 2: Installation (Only if required)
If you need to set up the environment, please execute the following commands. It is crucial to install the CUDA 12.1 specific PyTorch wheel to prevent torchvision::nms binary mismatch errors.

```bash
# 1. Install PyTorch configured for CUDA 12.1 (Crucial for Category C)
pip install torchvision==0.20.1 torchaudio==2.5.1 --index-url https://download.pytorch.org/whl/cu121

# Cell #1 in the Demo Notebook
!pip install -r ../requirements.txt
```

---

### Step 3: Mandatory Kernel Restart
**CRITICAL:** If you executed the installation in Step 2 (especially via Cell #1), you **MUST restart the Jupyter Kernel or Python Session** before running the rest of the notebook.

---

### Step 4: Pre-Run Checklist (Model Weights)
Before running the inference cells, please verify that the downloaded model folders are placed in the exact locations as instructed in Section 3:

* **Category A Path:** Group_33_AV/Category_A/Models_ML/
* **Category C Path:** Group_33_AV/Category_C/Models_ASL/
* **Verification:** Ensure the folder names match exactly and are **not nested** inside another redundant folder.

---

### Step 5: Execution
Once the environment is ready and the weights are in place, follow these steps:
1. Open **Category_A/Group_33_Demo_A.ipynb** and run the cells sequentially. 
   * *Note: Start from Cell #2 if you have already performed a kernel restart.*
2. Open **Category_C/Group_33_Demo_C.ipynb** and run the cells sequentially. 
   * *Note: Start from Cell #2 if you have already performed a kernel restart.*
3. The final prediction CSV files will be automatically generated in their respective category folders.

---

## 5. Generative AI Tools Declaration (Compliance with Section V)
In accordance with the assignment specifications, our team declares the use of **Gemini (Google AI)** for the following technical optimizations:

### Scope of AI Assistance:
1. **Training Diagnostic & Overfitting Analysis:**
   * Gemini was used to review training logs and intermediate results to diagnose potential **overfitting**. This assisted the team in verifying the model's convergence and the overall success of the fine-tuning sessions.

2. **Environment Optimization for Reproducibility:**
   * To ensure seamless execution on Python 3.12.7 and CUDA 12.1, we consulted Gemini to identify **optimal library versions**. 
   * This enabled us to resolve critical version conflicts and establish a stable environment pairing (torch 2.5.1 + vision 0.20.1 + numpy 1.26.4).

---

**End of Documentation.**

