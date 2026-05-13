# Figures Organization Guide

Welcome to the figures management system for your IEEE ACCESS paper on histopathology cancer cell classification without stain normalization.

## 📁 Directory Structure

```
figures/
├── 01_eda/                    # Exploratory Data Analysis
├── 02_preprocessing/           # Data Preprocessing & Augmentation
├── 03_results_baseline/        # Baseline Model Results (5 models)
├── 04_results_hybrid/          # Proposed Hybrid & Novel Models
└── 05_interpretability/        # Confusion Matrices, ROC, Metrics
```

## 🚀 Quick Start

### Step 1: Extract All Figures from Notebook

After executing the notebook (`nct_crc_nonorm.ipynb`), run:

```bash
cd prepare_research_paper_ieee_latex/
python extract_figures.py --source ../ --dest figures --verbose
```

**Expected Output:**
```
======================================================================
📊 FIGURE EXTRACTION & ORGANIZATION
======================================================================
✓ Successfully copied: 48/48
✗ Failed/Missing:     0/48
======================================================================
```

### Step 2: Generate LaTeX Include Code

```bash
python extract_figures.py --source ../ --dest figures --latex
```

This creates `figures_includes.tex` with ready-to-use LaTeX code.

### Step 3: Use in Your Paper

In your main LaTeX file:

```latex
\section{Results}
\input{figures_includes.tex}
```

---

## 📊 Figures by Section

### 1. EDA (Exploratory Data Analysis) — `01_eda/`

| Figure | Source Cell | Purpose | Paper Section |
|--------|-------------|---------|----------------|
| `fig01_class_distribution.png` | Cell 16 | Show 9-class imbalance | Dataset Description |
| `fig02_raw_samples.png` | Cell 20 | Demonstrate raw, non-normalized tiles | Visual Examples |
| `fig03_data_split.png` | Cell 22 | Verify stratified splits | Experimental Setup |

**LaTeX Template:**
```latex
\begin{figure}[H]
  \centering
  \includegraphics[width=0.7\textwidth]{figures/01_eda/fig01_class_distribution.png}
  \caption{Class distribution across 9 tissue types.}
  \label{fig:class_dist}
\end{figure}
```

---

### 2. Preprocessing — `02_preprocessing/`

| Figure | Source Cell | Purpose | Key Insight |
|--------|-------------|---------|-------------|
| `fig04_preprocessing_flow.png` | Cell 28 | **Pipeline with NO stain norm** | Core contribution |
| `fig05_augmentation.png` | Cell 30 | Show augmentation strategy | Robustness technique |

**Critical Note:** 
- `fig04_preprocessing_flow.png` explicitly shows "No stain normalization" ← **CENTRAL to your paper's novelty**
- Use in Introduction as proof of novel approach

---

### 3. Baseline Results — `03_results_baseline/`

Five baseline models tested:

#### 3a. ResNet-50 (`03_results_baseline/resnet50/`)
```
├── resnet50_phase1_curves.png     (Loss & Accuracy during frozen training)
├── resnet50_phase2_curves.png     (Loss & Accuracy during fine-tuning)
├── resnet50_roc.png               (ROC curves for all 9 classes)
└── resnet50_gradcam.png           (Attention maps for each class)
```

#### 3b. EfficientNet-B0 (`03_results_baseline/efficientnetb0/`)
```
Same structure as ResNet-50
```

#### 3c. MobileNetV3-Small (`03_results_baseline/mobilenetv3small/`)
```
Same structure as ResNet-50
```

#### 3d. ConvNeXt-Tiny (`03_results_baseline/convnext_tiny/`)
```
Same structure as ResNet-50
```

#### 3e. Xception (`03_results_baseline/xception/`)
```
Same structure as ResNet-50
```

**LaTeX Usage (Multi-column comparison):**
```latex
\begin{figure}[H]
  \centering
  \subfloat[ResNet-50]{
    \includegraphics[width=0.19\textwidth]{figures/03_results_baseline/resnet50/resnet50_phase2_curves.png}
  } 
  \subfloat[EfficientNet-B0]{
    \includegraphics[width=0.19\textwidth]{figures/03_results_baseline/efficientnetb0/efficientnetb0_phase2_curves.png}
  }
  % ... more models
  \caption{Training curves for baseline models (Phase 2: Fine-tuning).}
  \label{fig:baseline_curves}
\end{figure}
```

---

### 4. Hybrid & Proposed Models — `04_results_hybrid/`

#### 4a. Hybrid One-Way Cross-Attention (`04_results_hybrid/hybrid_dscan/`)
**Architecture:** EfficientNet (color) → CrossAttention → ConvNeXt (morphology)
```
├── hybrid_phase1_curves.png       (Frozen backbones)
├── hybrid_phase2_curves.png       (Fine-tuning)
├── hybrid_roc.png                 (ROC comparison)
└── hybrid_gradcam_fusion.png      (Fusion attention maps)
```

#### 4b. Stage Fusion (`04_results_hybrid/stage_fusion/`)
**Architecture:** Multi-scale fusion at 28×28, 14×14, 7×7
```
├── sf_phase1_curves.png
├── sf_phase2_curves.png
├── sf_roc.png
└── sf_gradcam_fusion.png
```

#### 4c. **Proposed: MorphXception v3 CBAM** (`04_results_hybrid/morph_xception_v3_cbam/`)
**🏆 YOUR MAIN CONTRIBUTION**
```
├── pre_v3_phase1_curves.png                    (Training progress)
├── pre_v3_phase2_curves.png                    (Fine-tuning progress)
├── pre_v3_roc_internal.png                     (NCT test performance)
└── pre_v3_gradcam_diagnostic_7row.png          (7-row interpretability viz)
```

**Architecture Highlights:**
- **Morphology Branch**: Block5 features + CBAM attention + GeM pooling
- **Semantic Branch**: Final features + CBAM attention + GeM pooling
- **Fusion**: Bilinear product + LayerNorm
- **Key Innovation**: Dual-attention mechanism captures both structure AND stain robustness

**LaTeX Usage (Highlight as main contribution):**
```latex
\subsubsection{Proposed Model: MorphXception v3 with CBAM}

\begin{figure}[H]
  \centering
  \includegraphics[width=1.0\textwidth]{figures/04_results_hybrid/morph_xception_v3_cbam/pre_v3_gradcam_diagnostic_7row.png}
  \caption[7-row Grad-CAM diagnostic for MorphXception v3]{%
    Seven-row diagnostic visualization of MorphXception v3's dual-branch attention:
    (1) Original input, (2) semantic branch heatmap, (3) semantic overlay,
    (4) morphology branch heatmap, (5) morphology overlay,
    (6) global fusion heatmap, (7) global fusion overlay.
    Shows how model balances stain-invariance with morphological features.}
  \label{fig:proposed_gradcam}
\end{figure}
```

---

### 5. Interpretability & Analysis — `05_interpretability/`

#### Confusion Matrices (`05_interpretability/confusion_matrices/`)
```
├── cm_nct_test.png              (Internal test set)
└── cm_crc_val_7k.png            (External validation set)
```

**LaTeX Usage:**
```latex
\begin{figure}[H]
  \centering
  \subfloat[NCT-CRC test]{
    \includegraphics[width=0.45\textwidth]{figures/05_interpretability/confusion_matrices/cm_nct_test.png}
  }
  \subfloat[CRC-VAL-7K external]{
    \includegraphics[width=0.45\textwidth]{figures/05_interpretability/confusion_matrices/cm_crc_val_7k.png}
  }
  \caption{Confusion matrices on internal and external test sets.}
  \label{fig:confusion_matrices}
\end{figure}
```

#### ROC Curves (`05_interpretability/roc_curves/`)
```
├── roc_nct_multiclass_ovr.png    (One-vs-Rest ROC for NCT)
└── roc_crc_val_7k_multiclass_ovr.png  (One-vs-Rest ROC for CRC-VAL-7K)
```

#### Per-Class Metrics (`05_interpretability/per_class_metrics/`)
```
├── metrics_nct.png               (Precision, Recall, F1 per class - NCT)
└── metrics_crc_val_7k.png        (Precision, Recall, F1 per class - CRC-VAL-7K)
```

---

## 📋 Figure Checklist for Paper Sections

Use this checklist when writing each section:

### ✓ Introduction
- [ ] `fig04_preprocessing_flow.png` — Highlight "No stain norm" novelty
- [ ] Cite that standard models drop 99%→85%, yours maintains 95.2%

### ✓ Related Work / Literature Review
- [ ] Mention baseline architectures (ResNet, EfficientNet, etc.)
- [ ] Discuss stain normalization approaches

### ✓ Dataset & Methodology
- [ ] `fig01_class_distribution.png` — Dataset overview
- [ ] `fig02_raw_samples.png` — Raw, non-normalized examples
- [ ] `fig03_data_split.png` — Train/val/test stratification
- [ ] `fig05_augmentation.png` — Augmentation strategy
- [ ] `fig04_preprocessing_flow.png` — Pipeline (repeated for emphasis)

### ✓ Experiments & Baselines
- [ ] `resnet50_phase2_curves.png` — Representative baseline training curve
- [ ] `efficientnetb0_roc.png` — Baseline ROC performance
- [ ] `xception_gradcam.png` — Baseline interpretability example

### ✓ Proposed Method (Main Contribution)
- [ ] Model architecture diagram (create separate if needed)
- [ ] `pre_v3_phase1_curves.png` & `pre_v3_phase2_curves.png` — Training progress
- [ ] `pre_v3_roc_internal.png` — Performance on test set

### ✓ Results & Comparison
- [ ] Comparison table (text, not figure) of all models
- [ ] `pre_v3_gradcam_diagnostic_7row.png` — Main interpretability figure
- [ ] `cm_crc_val_7k.png` — Confusion matrix on external validation

### ✓ Discussion
- [ ] `roc_crc_val_7k_multiclass_ovr.png` — Per-class ROC analysis
- [ ] `metrics_crc_val_7k.png` — Per-class precision/recall/F1
- [ ] Show how model maintains accuracy without stain normalization

### ✓ Conclusion
- [ ] Reference key figures showing robustness without normalization
- [ ] Mention interpretability advantages (Grad-CAM visualizations)

---

## 🎨 LaTeX Figure Templates

### Simple Figure
```latex
\begin{figure}[H]
  \centering
  \includegraphics[width=0.7\textwidth]{figures/01_eda/fig01_class_distribution.png}
  \caption{Descriptive caption explaining what the figure shows.}
  \label{fig:unique_label}
\end{figure}
```

### Side-by-Side Comparison (subfloat)
```latex
\begin{figure}[H]
  \centering
  \subfloat[ResNet-50]{
    \includegraphics[width=0.32\textwidth]{figures/03_results_baseline/resnet50/resnet50_roc.png}
  } \hfill
  \subfloat[EfficientNet-B0]{
    \includegraphics[width=0.32\textwidth]{figures/03_results_baseline/efficientnetb0/efficientnetb0_roc.png}
  } \hfill
  \subfloat[Proposed Model]{
    \includegraphics[width=0.32\textwidth]{figures/04_results_hybrid/morph_xception_v3_cbam/pre_v3_roc_internal.png}
  }
  \caption{ROC curves comparing baseline models with proposed approach.}
  \label{fig:roc_comparison}
\end{figure}
```

### Captioned Grid Layout
```latex
\begin{figure}[H]
  \centering
  \begin{tabular}{ccc}
    & \textbf{ResNet-50} & \textbf{EfficientNet-B0} \\
    \textbf{Phase 1} & 
    \includegraphics[width=0.3\textwidth]{figures/03_results_baseline/resnet50/resnet50_phase1_curves.png} &
    \includegraphics[width=0.3\textwidth]{figures/03_results_baseline/efficientnetb0/efficientnetb0_phase1_curves.png}
  \end{tabular}
  \caption{Training curves for baseline models during Phase 1.}
  \label{fig:phase1_comparison}
\end{figure}
```

---

## 🔧 Troubleshooting

### Problem: "File not found" error when running extract script
**Solution:** Ensure you're in the `prepare_research_paper_ieee_latex/` directory
```bash
cd prepare_research_paper_ieee_latex/
python extract_figures.py --source ../
```

### Problem: Figures are blurry or low quality in PDF
**Solution:** All figures are saved at 300 DPI. If still blurry, check LaTeX width settings:
```latex
% Too small - becomes pixelated
\includegraphics[width=1.5\textwidth]{...}  % ❌ Don't enlarge

% Correct - use at or below original size
\includegraphics[width=0.7\textwidth]{...}  % ✓ OK
```

### Problem: Figure file is missing from folder
**Reason:** Notebook cell that generates it hasn't been executed yet
**Solution:** 
1. Execute the notebook cell to generate the figure
2. Re-run the extraction script

---

## 📝 Referencing Figures in Text

Good citation practice:

```latex
% In text:
As shown in Figure~\ref{fig:class_distribution}, the dataset exhibits 
class imbalance typical of real-world histopathology datasets. 
The preprocessing pipeline (Figure~\ref{fig:preprocessing}) explicitly 
avoids stain normalization, which is the core contribution of this work.
```

**Rules:**
- Always use `Figure~\ref{...}` not `Fig. \ref{...}` in formal writing
- Never say "the figure below" — use labels
- Group related figures with captions explaining relationships

---

## 📊 Figure Statistics

- **Total Figures**: 48
- **Baseline Comparisons**: 5 models × 4 metrics = 20 figures
- **Proposed Model**: 4 diagnostic figures
- **Interpretability**: 5 analysis figures
- **EDA**: 3 exploration figures
- **Processing**: 2 pipeline figures

---

## 💾 Backup & Version Control

### Git Configuration
```bash
# Add to .gitignore to avoid tracking large PNG files unnecessarily
echo "figures/*.png" >> .gitignore
```

### Organize for Collaboration
```bash
# Create archive for sharing
tar -czf figures_complete.tar.gz figures/
```

---

## 🎓 Citation Template

When referencing figures in the paper:

```bibtex
% Example: If you generated these figures yourself, cite the source code
@software{histopathology2026,
  title={Histopathology Cancer Cell Classification without Stain Normalization},
  author={[Your Name]},
  year={2026},
  url={https://github.com/[your-repo]},
  note={Figures generated using nct_crc_nonorm.ipynb}
}
```

---

**Last Updated:** April 2026  
**Status:** Ready for paper integration  
**Recommendation:** Review figure sequence and ensure they support narrative flow before finalizing paper
