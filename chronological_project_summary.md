# Chronological Project Summary

## Project Goal Context

In this project I've studyed **cross-subject motor imagery EEG classification** on **BNCI2014_001** using the **LeftRightImagery** paradigm.

The main goal is to understand whether low-performing subjects can be improved while staying close to a classical **CSP + LDA** pipeline.

The project focuses especially on subjects:

- 2
- 5
- 6

These subjects consistently appeared among the weakest performers in the subject-independent experiments.

---

## Dataset and Task

- **Dataset:** BNCI2014_001
- **Paradigm:** LeftRightImagery
- **Subjects:** 9
- **Sessions:** 2
- **Classes used:** left hand vs right hand only

---

## Summary

### 1. Baseline notebook: `02_baseline_csp_lda.ipynb`

#### Setup
- Pipeline: **CSP + LDA**
- Band: **8–32 Hz**
- CSP components: **8**
- Evaluation: **cross-subject**
- Interpretation: fully subject-independent baseline

#### Result
- Mean score: **0.7719**
- Std: **0.1625**
- Weak subjects: **2, 5, 6**

#### Analysis
The baseline was already fairly strong overall, but performance varied heavily by subject.  
This established the project problem: some subjects are consistently much harder than others.

---

### 2. Alternative pipeline: `03_ce_tsp_lr.ipynb` (Riemannian + Logistic Regression)

#### Setup
- Changed from CSP-based features to a Riemannian pipeline
- Purpose: comparison against CSP + LDA

#### Result
- Mean score: **0.7765**
- Std: **0.1570**
- Weak subjects remained mostly weak

#### Analysis
It showed that a different model family could produce similar performance, but it did **not** produce a clear improvement over the baseline.
I treated it as an alternative comparison rather than a real “improved CSP + LDA” method.

---

### 3. Euclidean Alignment experiment: `03_ea_csp_lda.ipynb`

#### Setup
- Pipeline: **EA + CSP + LDA**
- Band: **8–32 Hz**
- CSP components: **8**
- Evaluation: **cross-subject**
- Purpose: tried to improve the CSP + LDA

#### Result
- Mean score: **0.7719**
- Std: **0.1625**
- Weak subjects: **2, 5, 6**
- Same outcome as baseline

#### Analysis
Euclidean Alignment did not improve performance.  
The results matched the original baseline so this experiment was not useful as the improvement path.

---

### 4. Filter-bank CSP experiment: `03_fbcsp_lda.ipynb`

#### Setup
- Pipeline: **FBCSP + LDA**
- Bands tested inside the notebook:
  - 8-12
  - 12-16
  - 16-20
  - 20-24
  - 24-28
  - 28-32
- CSP components per band: **2**
- Evaluation: **cross-subject**

#### Result
- Mean score: **0.6308**
- Std: **0.0846**
- Weak subjects: **2, 5, 6**

#### Analysis
This simplified FBCSP implementation performed **worse** than the original baseline.
It suggested that merely splitting the spectrum into several small bands did not help in this setup.

---

### 5. Baseline tuning sweep: `04_csp_band_tuning.ipynb`

#### Setup
This notebook performed a controlled sweep over the original CSP + LDA baseline.

Bands tested:
- 8–30
- 8–32
- 8–35
- 10–30
- 12–30

CSP components tested:
- 4
- 6
- 8
- 10

#### Best overall configuration
- **Band:** 8–35 Hz
- **CSP components:** 10
- **Mean score:** 0.7757

#### Best bottom-3 configuration for subjects 2, 5, 6
- **Band:** 8–35 Hz
- **CSP components:** 10
- **Bottom-3 mean:** 0.6107

#### Subject-specific best configs
- Subject 2: **8–32 / CSP 6**
- Subject 5: **12–30 / CSP 6**
- Subject 6: **12–30 / CSP 8**

#### Analysis
This was the most useful tuning experiment.

Main findings:
- The original baseline was already near the top of the search space.
- Small global gains were possible.
- The best single global replacement for the baseline was:
  - **8–35 Hz + CSP 10 + LDA**
- Weak subjects did **not** all prefer the same configuration.

This means subject variability remained a real issue even after tuning.
