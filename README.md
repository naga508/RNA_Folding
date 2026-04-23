# Template-Free RNA 3D Folding (Part 2) — README

This repository/package contains **two Jupyter notebooks** used for the course project:
**Machine Learning for Template-Free RNA 3D Structure Prediction** (Pattern Recognition).

The notebooks implement **end-to-end, single-trigger pipelines** that (when run top-to-bottom) load the Kaggle
Stanford RNA 3D Folding (Part 2) data, generate **5 candidate 3D structures per RNA**, and write a `submission.csv`
in the required format.

---

## Files in this submission

1) **`stable-0-413-without-hash (1).ipynb`**  
   **Protenix + TBM hybrid inference** notebook. Includes a `main()` entry point and a built-in *Local vs Kaggle* mode.
   - Uses the Kaggle dataset: `stanford-rna-3d-folding-2`
   - Uses Protenix code/weights from: `qiweiyin/protenix-v1-adjusted`
   - Produces: `submission.csv` (default: `/kaggle/working/submission.csv`)

2) **`stanford-rna-3d-extra-tbm-templates.ipynb`**  
   **Template-Based Modeling (TBM) ensemble** notebook using an external template library.
   - Uses the Kaggle dataset: `stanford-rna-3d-folding-2`
   - Uses external template datasets (Kaggle):
     - `odat1248/rnatbm2025-set`
     - `odat1248/rnatbm-templates-20250521-re`
   - Produces: `submission.csv` in the working directory

---

## Hardware / runtime expectations

- **GPU is strongly recommended.** These pipelines are designed for Kaggle-style GPU execution.
- **Local sanity check:** `stable-0-413-without-hash (1).ipynb` automatically switches to *LOCAL mode* (default: only
  the first `LOCAL_N_SAMPLES=2` test sequences) when `KAGGLE_IS_COMPETITION_RERUN` is not set.
- **Full test run:** depends on hardware and dataset size; typically **tens of minutes** on a Kaggle GPU.
  The notebook prints progress (e.g., via `tqdm`) while running.

---

## Dependencies

### Python packages (minimum)
- `python>=3.10`
- `numpy`, `pandas`
- `torch` (GPU-enabled recommended)
- `tqdm`
- `biopython` (used for sequence alignment via `PairwiseAligner`)

### Kaggle-specific installs
The TBM notebook includes a Kaggle offline wheel install:
```bash
!pip install --no-index /kaggle/input/biopython-cp312/biopython-1.86-*.whl
```
When running **locally**, replace that line with:
```bash
pip install biopython
```

---

## Dataset setup

### Option A (Recommended): Run on Kaggle
1. Open Kaggle → **Stanford RNA 3D Folding (Part 2)** competition.
2. Create a **new Notebook** (GPU enabled).
3. Attach datasets via **Add data**:
   - `stanford-rna-3d-folding-2`
   - For Protenix notebook: `qiweiyin/protenix-v1-adjusted`
   - For TBM notebook: `odat1248/rnatbm2025-set`, `odat1248/rnatbm-templates-20250521-re`
4. Upload one of the notebooks (or copy-paste the notebook content).
5. Run **Run All**.
6. Confirm `submission.csv` appears in `/kaggle/working/` and submit.

### Option B: Run locally (Instructor machine)
> **Important:** Both notebooks use Kaggle default paths (`/kaggle/input/...`). For local execution you must download
> the datasets and update path variables at the top of the notebooks.

Suggested local folder layout:
```
project_root/
  data/stanford-rna-3d-folding-2/        # train_sequences.csv, train_labels.csv, test_sequences.csv, ...
  external/protenix-v1-adjusted/         # Protenix-v1 code/weights (for notebook 1)
  external/tbm_templates_set1/           # odat1248/rnatbm2025-set (for notebook 2)
  external/tbm_templates_set2/           # odat1248/rnatbm-templates-20250521-re (for notebook 2)
  stable-0-413-without-hash (1).ipynb
  stanford-rna-3d-extra-tbm-templates.ipynb
```

Then update these variables:

**Notebook 1 (`stable-0-413-without-hash (1).ipynb`)**
- `DATA_BASE` (currently `/kaggle/input/stanford-rna-3d-folding-2`)
- `DEFAULT_CODE_DIR` (currently points into `/kaggle/input/datasets/qiweiyin/protenix-v1-adjusted/...`)

**Notebook 2 (`stanford-rna-3d-extra-tbm-templates.ipynb`)**
- `DATA_PATH` (currently `/kaggle/input/stanford-rna-3d-folding-2/`)
- Template dataset paths (the notebook references `/kaggle/input/datasets/odat1248/...`)

---

## How to run (single-trigger, sequential)

### Method 1: In Jupyter (simplest)
Open the notebook and click:
- **Kernel → Restart & Run All**

This runs the notebook sequentially without interruption and produces `submission.csv`.

### Method 2: Command line execution (recommended for reproducibility)
From the folder containing the notebook:
```bash
jupyter nbconvert --to notebook --execute "stable-0-413-without-hash (1).ipynb" --output executed_notebook1.ipynb
jupyter nbconvert --to notebook --execute "stanford-rna-3d-extra-tbm-templates.ipynb" --output executed_notebook2.ipynb
```

---

## Outputs

Both notebooks produce:
- `submission.csv` containing:
  - id / residue identifiers
  - predicted coordinates for **5 candidates** (`x_1..x_5`, `y_1..y_5`, `z_1..z_5`) in the required format

Optional intermediate artifacts (depending on notebook settings):
- console logs with progress + timing
- validation prints / sanity checks

---

## Progress reporting
- Notebook 1 uses `tqdm` for progress bars and prints whether it is in **LOCAL** or **KAGGLE** mode.
- Notebook 2 prints progress messages while building template matches and producing 5 candidates.

If desired for the final submission, add/keep explicit prints such as:
- “Loading data…”
- “Training completed”
- “Generating submission…”
- “Saved submission.csv”

---

## Code borrowing / attribution (academic integrity)
The Protenix notebook explicitly references and is adapted from Kaggle notebooks. Please **cite these sources** in
your final report (and/or in a “Borrowed Code” section):

- `qiweiyin/protenix-v1-inference-2026`
- `nihilisticneuralnet/0-409-stanford-rna-folding-2-protenix-template`
- `alexxanderlarko/protenix-v1`

The TBM notebook states it is based on a prior LB notebook + external template library. If additional sources were
used, add them here with proper citation.

---

## Reproducibility notes
- Both notebooks set a random seed (e.g., `seed=42`) for repeatability.
- Exact results may still vary slightly across hardware/driver versions, especially when using GPU kernels.

---

## Troubleshooting
- **Missing `/kaggle/input/...` paths locally:** download datasets and update `DATA_BASE` / `DATA_PATH` variables.
- **Biopython install errors:** ensure `pip install biopython` locally; on Kaggle, keep the offline wheel install.
- **CUDA / torch issues:** install a PyTorch build compatible with your GPU + CUDA version.

