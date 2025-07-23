# Bioinformatics Paper Recommender & Chatbot System

This project presents a full-stack machine learning pipeline to power an intelligent chatbot system for bioinformatics literature. It integrates classification, clustering, and recommendation engines with a lightweight API and visual frontend (via N8N) to allow users to query research papers by topic, author, year, and semantic similarity.

---

## Dataset Description

The dataset consists of 1,000 research papers sampled from Web of Science, categorized into five bioinformatics areas:

- Gene Expression Analysis
- Sequence Classification and Alignment
- Protein Structure and Function Prediction
- Biological Image Analysis
- Disease Outcome Prediction

Each entry contains:
- Title
- Abstract
- Publication Year
- Authors
- Source Title
- Times Cited
- Document Type
- Author Keywords



## Models and Evaluation

### **1. Text Classification**
- **Model:** Multinomial Naive Bayes
- **Features:** BoW + Bigrams
- **Accuracy:** 94.70%
- **ROC-AUC:** 0.9951
- **Confusion Analysis:** Most confusion occurred between protein and sequence categories.
- **Top Misclassified Words:** "fold", "domain", "alignment", "genome"

### **2. Text Clustering**
- **Model:** K-Means on TF-IDF + PCA
- **Reason for Choice:** Unlike Ward or Spectral clustering, K-Means supports `.predict()` for API inference.
- **Cluster Quality:** Silhouette Score = 0.722
- **Cluster Themes:**

| Cluster | Top Terms |
|--------|-----------|
| 0 | image, learning, analysis, imaging, cell, biological, segmentation |
| 1 | disease, prediction, patient, heart, risk, clinical, outcome |
| 2 | gene, expression, cancer, DEG, biomarkers, immune |
| 3 | protein, structure, prediction, function, fold, interaction |
| 4 | sequence, alignment, genome, virus, classifier |

- **Mapping Accuracy:** 88.4% of papers matched expected clusters.
  - E.g., 193/200 biological image papers were in Cluster 0.
  - Sequence classification spread slightly across Cluster 3 and 4.

### **3. Recommender Engine**
- **Type:** Hybrid Recommender
- **Components:**
  - **Content Similarity:** Cosine distance (Title + Abstract TF-IDF)
  - **Metadata Filtering:** Year, Document Type, Authors
  - **Citation-aware Sorting:** Higher cited papers ranked higher

- **Top-N Output:**
  - Sorted by: `Final_Similarity × Citation Count`
  - Efficient and interpretable for frontend use

---

##  API

The Flask-based API powers dynamic integration with N8N.

### Endpoints:
- `POST /classify`: Returns predicted category of a paper
- `POST /cluster_predict`: Returns cluster ID and theme
- `POST /recommend`: Returns Top-N recommended papers
- `GET /health`: Health check

## Environment & Tools

- **Python version:** 3.8+

- **Platforms:** Google Colab / Jupyter Notebook

- **Core Libraries:**
  - `pandas`, `numpy`, `scikit-learn`, `nltk`, `gensim`, `spacy`
  - `matplotlib`, `seaborn`, `joblib`, `pickle`
  - `shap`, `xgboost`, `transformers` (for future use)






