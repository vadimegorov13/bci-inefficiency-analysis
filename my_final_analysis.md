# Analyzing Subject Variability in Cross-Subject Motor Imagery EEG Classification

## Introduction

Brain-computer interface systems based on EEG motor imagery can work well for some users but much worse for others. This subject-to-subject variability makes practical BCI systems less reliable and less accessible. In this project, I studied whether classical data mining methods could reduce that variability in a cross-subject motor imagery classification setting.

This project addresses a meaningful BCI problem called BCI illiteracy, uses a real public dataset, and evaluates multiple data mining approaches to see whether weak-performing subjects can be improved.

BCI illiteracy is a phenomenon that affects roughly 30% of the population. The term refers to the fact that some individuals produce brain signals that are much harder to decode reliably in a brain-computer interface system, even when they perform the same task as higher-performing users. In practice, this means that a BCI model may work well for some subjects but fail for others, which creates an important challenge for fairness, usability, and real-world adoption.

---

## Dataset and Task

The dataset used in this project was BNCI2014_001, and the task was restricted to left-hand vs right-hand motor imagery. The dataset contains 9 subjects and 2 sessions, which makes it a manageable but realistic benchmark for cross-subject EEG classification. The main focus of the project was subject-independent decoding, meaning a model should generalize across users rather than only within one person.

---

## Problem

Can classical EEG feature extraction and transfer-style methods improve cross-subject performance, especially for low-performing subjects?

The analysis consistently identified Subjects 2, 5, and 6 as the weakest users in the subject-independent experiments, so these subjects became the main focus of the later tuning and transfer analysis.

---

## Methods

The project began with a classical CSP + LDA baseline. After establishing the baseline, several follow-up experiments were conducted:

- EA + CSP + LDA, to test whether Euclidean Alignment improved transfer
- FBCSP + LDA, to test a filter-bank CSP variant band and CSP-component tuning, to see whether the original baseline could be improved through better parameter choices

---

## Baseline Results

The original subject-independent baseline used 8–32 Hz, 8 CSP components, and LDA. This baseline achieved a mean score of 0.7719 with standard deviation 0.1625. The lowest-performing subjects were 2, 5, and 6.

This was an important result because it showed two things at once. First, the baseline was already reasonably strong overall. Second, there was clear user-specific variability, which justified the rest of the project.

---

## Exploratory Experiments

The first exploratory extension was EA + CSP + LDA. In this setup, Euclidean Alignment did not improve the baseline in any meaningful way. The mean score remained 0.7719, and the weakest subjects stayed the same.

The second exploratory extension was FBCSP + LDA. This simplified filter-bank implementation performed substantially worse, with a mean score of 0.6308. The same weak subjects remained among the lowest performers.

These two experiments were still useful because they showed that not every transfer or spectral extension automatically improves cross-subject decoding. In this project, both methods failed to provide a clear improvement over the original classical baseline.

---

## Baseline Tuning Results

The most useful experiment in the project was a controlled tuning sweep over the original CSP + LDA baseline. Frequency bands and CSP component counts were varied systematically. This sweep found that the best global configuration was:

Band: 8–35 Hz
CSP components: 10
Mean score: 0.7757

This result improved slightly over the original baseline mean of 0.7719. The same tuned configuration was also best on average for the weak-subject group (2, 5, 6), with a bottom-3 mean of 0.6107.

However, the tuning sweep also revealed an important limitation: the weak subjects did not all prefer the same settings. Subject 2, Subject 5, and Subject 6 each had different best local configurations. That means the problem is not fully solved by one global hyperparameter choice. Instead, subject variability remains the main unresolved issue.

---

## Discussion

The main lesson from this project is that subject variability is the true challenge in this dataset. A classical CSP + LDA model already performs reasonably well overall, and modest improvements can be obtained through tuning. However, the weak subjects remain weak, and no single method tested here clearly solved their problem.

---

## Conclusion

This project analyzed cross-subject motor imagery EEG classification using classical CSP-based methods on BNCI2014_001. The original baseline was already strong, but it showed clear low-performing subjects. Euclidean Alignment and a simplified FBCSP implementation did not improve the subject-independent baseline. A controlled tuning sweep found that 8–35 Hz with 10 CSP components was the best global classical configuration.

Overall, the project demonstrates that cross-subject EEG classification is strongly limited by subject-specific variability. Modest gains are possible, but low-performing users remain the hardest part of the problem.