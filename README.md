# HAI_Assignment3_AI_Front

Frontend web interface for the **AI-Assisted Condition** of the Skin Lesion Triage Human-Subjects Study (Assignment 3).

## Overview
Participants inspect 10 randomly sampled smartphone images of skin lesions from the curated 200-image PAD-UFES-20 study dataset (plus 1 embedded attention-check trial). For each image, the interface displays the **AI Model Suggestion** (**Malignant** or **Benign**, without displaying a confidence percentage), and the participant selects **Agree**, **Invert**, or **Defer** (when image quality is insufficient).

## Repository Structure
* **`index.html`**: Main 3-column interface matching the study wireframe (Top Instructions bar, Left 6 Biopsy-Proven Lesion Examples, Center Medical Image viewer + AI Suggestion banner, Right ABCDE General Lesion Guidance, and Bottom 5-button action bar with `Agree`, `Invert`, `Defer`).
* **`styles.css`**: Responsive, single-screen layout styling for the AI-Assisted study interface, AI verdict badge, and modals.
* **`app.js`**: Study workflow logic:
  * Loads 10 random trials from the Python backend (`GET /api/trials?condition=ai&n=10`) with automatic local CSV fallback (`study_dataset_metadata.csv`).
  * Calls `POST /api/predict` on the Python backend to obtain live AI model predictions (+3 bonus points), with fallback to `study_dataset_metadata.csv`.
  * Embeds 1 attention-check trial at the midpoint (Trial 6 of 11).
  * Maps `Agree` / `Invert` / `Defer` to the final clinical verdict and tracks `response_time_seconds` per trial.
  * Collects post-study subjective experience ratings (perceived confidence, task difficulty, and AI helpfulness) and submits results to the Python/Supabase backend (`POST /api/submit`).
* **`study_dataset_metadata.csv`**: Curated 200-trial metadata from PAD-UFES-20.
* **`study_images/`**: Optimized 200 smartphone skin lesion `.png` images evaluated in the study.
* **`example_images/`**: 6 biopsy-proven reference images from HAM10000 (4 malignant: `example_mel.jpg`, `example_bcc.jpg`, `example_akiec.jpg`, `example_scc.jpg`; 2 benign: `example_nv.jpg`, `example_sek.jpg`) displayed in the left reference panel.

## Running Locally
```bash
python3 -m http.server 3001
```
Then open `http://localhost:3001` in your browser.

## Dataset Sources & Citations
* **PAD-UFES-20 (`study_images/` & `study_dataset_metadata.csv`):**
  * **Source:** [https://data.mendeley.com/datasets/zr7vgbcyr2/](https://data.mendeley.com/datasets/zr7vgbcyr2/)
  * **Citation:** Pacheco, A. G. C., Lima, G. R., Salomão, A. S., Krohling, B., Biral, I. P., de Angelo, G. G., Alves Jr, F. C. R., Esgario, J. G. M., Simora, A. C., Castro, P. B. C., Rodrigues, F. B., Frasson, P. H. L., Krohling, R. A., Knidel, H., Santos, M. C. S., Espírito Santo, R. B., Macedo, T. L. S. G., Canuto, T. R. P., & de Barros, L. F. S. (2020). *PAD-UFES-20: A skin lesion dataset composed of patient data and clinical images collected from smartphones* (Version 1) [Data set]. Mendeley Data. https://doi.org/10.17632/zr7vgbcyr2.1
* **HAM10000 (`example_images/` & AI Model Training Data):**
  * **Source:** [https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/DBW86T](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/DBW86T)
  * **Dataset Citation:** Tschandl, P. (2018). *The HAM10000 dataset, a large collection of multi-source dermatoscopic images of common pigmented skin lesions* (Version V4) [Data set]. Harvard Dataverse. https://doi.org/10.7910/DVN/DBW86T
  * **Companion Article:** Tschandl, P., Rosendahl, C., & Kittler, H. (2018). The HAM10000 dataset, a large collection of multi-source dermatoscopic images of common pigmented skin lesions. *Scientific Data*, 5, 180161. https://doi.org/10.1038/sdata.2018.161

