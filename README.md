# 🧹 OCR Text Cleaning with Small Language Models

This project aims to **automatically clean noisy OCR-generated text** using compact language models (LLMs). We use the OCR version of *The Vampyre* by John William Polidori as our case study.

We evaluate three individual LLMs and one hybrid approach for text correction, using two complementary evaluation methods:
- **LLM-as-a-Judge** (with **Gemini** as the evaluator),
- **Manual annotations** (performed by ourselves).

---

## 🎯 Objectives

1. **Assess the reliability** of using LLMs as evaluators ("LLM-as-a-Judge") for OCR text correction.
2. **Explore the benefit of combining models** to improve text restoration performance.

---

## 📁 Project Structure

### Paper

`MNLP_Homework_2_Pythoms.pdf` : The paper we wrote, associated to this work

### 🗂 Notebooks

- `00_Data_extraction.ipynb`  
  Unzips the provided data archive containing OCR text.

- `01_Collect_data.ipynb`  
  Explores and documents the structure of the raw OCR dataset.

- `02_Models.ipynb`  
  Applies three different LLMs (T5, MarianMT, BART) and a combined approach (MarianMT → T5) to correct the OCR text.  
  **Input**: Raw OCR text  
  **Output**: Cleaned text in CSV format: `{model}_correction.csv`

- `03_LLM_as_a_judge_{model}.ipynb`  
  Evaluates each corrected text using Gemini (LLM-as-a-Judge), which assigns scores (1–5) across text segments.  
  **Output**: `{model}_gemini_scores_all_texts.csv`

- `04_Students_as_a_judge_{model}.ipynb`  
  Manual annotation of the same text segments, based on the same evaluation criteria.  
  **Output**: `{model}_manual_scores.csv`

- `05_Correlation.ipynb`  
  Analyzes the agreement between Gemini and human scores to answer our research questions.

- `06_Output_files.ipynb`  
  Prepares final data outputs for submission: corrected texts, Gemini and manual annotations, and a 5,000-token extract from the best-performing model.

- `final_notebook.ipynb`  
  A full project summary and walkthrough — designed for instructors or reviewers.

---

### 📂 Folder Structure

- `data/`  
  Contains the raw OCR dataset.

- `output_files/`  
  Stores all intermediate and final outputs generated throughout the project.

- `plots/`  
  Contains visualizations created during the correlation analysis.

---

## 📊 Results & Insights

### 🔍 What’s the Best Model? Can LLM Combinations Boost Performance?

After extensive experimentation, we observed the following:

- **Translation models** like *MarianMT* are **ineffective for OCR correction** — they tend to distort the intended meaning rather than correct typographical errors.
- **T5** provides **moderate gains**, but not enough to outperform simpler baselines.
- **BART** consistently delivers the **best results across all metrics**.

To objectively assess the output quality, we used **ROUGE** scores — a standard metric for measuring overlap with ground-truth text.

| **Method**               | **ROUGE-1** | **ROUGE-2** | **ROUGE-L** |
|--------------------------|-------------|-------------|-------------|
| Back-Translation         | 0.7214      | 0.4295      | 0.6409      |
| **BART**                 | **0.8547**  | **0.7738**  | **0.8430**  |
| T5                       | 0.8296      | 0.7192      | 0.8185      |
| OCR (baseline)           | 0.8026      | 0.6577      | 0.8013      |
| Back-Translation + T5    | 0.7226      | 0.4541      | 0.6388      |

📌 **Takeaway**:  
**BART is the most effective model** for OCR post-correction in this setup. Interestingly, **model combinations did not outperform** standalone models.

---

### 🧠 LLM-as-a-Judge: Can We Trust Models Like Gemini to Evaluate Text?

We explored whether an LLM (Gemini) can reliably score and rank model outputs.

- **Gemini tends to overrate the raw OCR output**, often assigning it higher quality than warranted.
- Its scoring is **less nuanced**, avoiding extreme ratings.
- It **correctly identified the best (BART)** and **worst (Back-Translation)** performing models.
- However, it **misjudged T5**, ranking it below raw OCR — contradicting both human evaluation and ROUGE scores.

📌 **Takeaway**:  
While Gemini can spot clear differences in quality, it **lacks the sensitivity needed for finer distinctions**. It’s useful for quick assessments but **shouldn’t be relied on for detailed model evaluation**.

---

## 📬 Contact

If you have questions or would like to collaborate, feel free to reach out:

- **Thomas Garnier** – tgarnier067@gmail.com  
- **Thomas Chen** – thomaschen178@gmail.com
