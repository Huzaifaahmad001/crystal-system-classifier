# Crystal System Classifier
 
Composition-based crystal symmetry prediction using Random Forest. Reproduces and
extends the method published in:
 
> Li Y, Dong R, Yang W, et al. "Composition based crystal materials symmetry
> prediction using machine learning with enhanced descriptors."
> *Computational Materials Science*, 2021, 198: 110686.
> Original code and data: https://github.com/Yuxinya/SG_predict
 
## What this does
 
Given only a chemical formula — no structure, no diffraction data — predicts the
material's crystal symmetry (space group) using a Random Forest trained on 256
Magpie composition descriptors computed with matminer.
 
## Results
 
**Held-out test (cubic system, 20% split):**
 
| Metric | This repo | Published (10-fold CV) |
|---|---|---|
| Accuracy | 0.957 | 0.961 ± 0.006 |
| F1 (weighted) | 0.956 | 0.959 ± 0.006 |
| MCC | 0.940 | 0.945 ± 0.008 |
 
**Extended to all seven crystal systems** (not demonstrated in the original repo):
 
| Crystal system | Accuracy | Samples |
|---|---|---|
| Cubic | 0.957 | 17,367 |
| Hexagonal | 0.898 | 8,201 |
| Tetragonal | 0.847 | 12,675 |
| Triclinic | 0.822 | 9,440 |
| Trigonal | 0.807 | 9,429 |
| Orthorhombic | 0.752 | 22,392 |
| Monoclinic | 0.691 | 23,024 |
 
Notably, monoclinic has the most training data of any system and still scores
lowest — accuracy does not track cleanly with sample count here.
 
**Algorithm comparison:** Random Forest 0.957 vs. XGBoost 0.954 (near tie).
 
**Hyperparameter search (GridSearchCV):** independently rediscovered the paper's
own settings (100 trees, max_features=80) as optimal, best CV accuracy 0.953.
 
**Confusion matrix finding:** the model's single most common mistake is confusing
space groups 216 and 225 with each other (21 and 12 misclassifications
respectively) — far ahead of any other error pair.
 
## What's original here vs. what's from the paper
 
The method and the cubic-system training data belong to Li et al. (2021). This
repository independently reproduces their result, then adds: full seven-system
training (above), feature-importance analysis, a live prediction pipeline for new
formulas, an XGBoost comparison, a hyperparameter search, and a confusion-matrix
error analysis — none of which the original repository demonstrates.
 
## Repository contents
 
- `crystal_system_classifier.ipynb` — the full notebook, runnable top to bottom
  in Google Colab.
 
## Data
 
- `cubic.csv` is fetched automatically by the notebook from the original authors'
  repository (github.com/Yuxinya/SG_predict) — no manual download needed.
- The full seven-system dataset (102,528 rows, ~105–136 MB) is **not included in
  this repository** — it exceeds GitHub's upload limits. To reproduce the
  seven-system results, obtain the data file and upload it to your own Google
  Drive as `data.xlsx`; the notebook loads it directly from Drive.
 
## How to run
 
1. Open `crystal_system_classifier.ipynb` in Google Colab.
2. Run the cells in order from top to bottom.
3. For the seven-system section, make sure `data.xlsx` is in your Google Drive
   first and approve the Drive-access prompt when it appears.
 
## Citation
 
If you use this code, please cite the original paper:
 
```
Li Y, Dong R, Yang W, et al. Composition based crystal materials symmetry
prediction using machine learning with enhanced descriptors. Computational
Materials Science, 2021, 198: 110686.
```
