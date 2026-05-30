
**“Data-driven validation of autonomic and cognitive readiness-state basins using public RR-interval and cognitive-control databases.”**

The key caution is that I would avoid claiming “brain states” too strongly at this stage. The safer phrase is **criticality-like cognitive/autonomic state classifications**. Your own specs make that boundary clear: the cognitive classifier estimates criticality-like cognitive dynamics from behaviour, not direct neural criticality, and the body classifier estimates criticality-like autonomic dynamics from RR structure rather than diagnosing physiology or brain state.  

---

## A. Candidate journals

| Journal                                             | Fit                                                                                                                                                                                                                                                                                 | Submission angle                                                                                              |
| --------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **Behavior Research Methods**                       | Best fit if positioned as a reusable computational method for extracting latent cognitive/autonomic state basins from public behavioural and physiological datasets. The journal is explicitly focused on methods, techniques and tools in experimental psychology. ([Springer][1]) | Strong first target if you provide open Python code, reproducible notebooks and a clear validation framework. |
| **Psychophysiology**                                | Strong if the paper emphasises autonomic–cognitive state coupling, RR dynamics, cognitive control and psychophysiological state modelling. It covers interrelationships between physiological and psychological aspects of brain and behaviour. ([Wiley Online Library][2])         | Best if the RR and cognitive-control parts are both retained and you include ECG/behaviour bridge analyses.   |
| **International Journal of Psychophysiology**       | Strong for an interdisciplinary psychophysiology paper integrating behavioural and physiological state models. The journal aims to integrate neurosciences and behavioural sciences. ([ScienceDirect][3])                                                                           | Good alternative to Psychophysiology, especially if the RR methods are central.                               |
| **Biological Psychology**                           | Good if framed around physiological aspects of psychological states and processes, especially cognitive effort, stress, attention and autonomic regulation. ([ScienceDirect][4])                                                                                                    | Good if you strengthen the mind–body and autonomic-state interpretation.                                      |
| **Cognitive Research: Principles and Implications** | Good if the paper focuses more on cognitive-control task data, latent control states and applied cognitive measurement. It publishes empirical/theoretical work across cognition with use-inspired emphasis. ([Springer][5])                                                        | Best if RR is secondary or in a companion paper.                                                              |
| **PLOS ONE**                                        | Good broad-scope target for a rigorously conducted open-data validation study, including null or mixed findings. PLOS ONE evaluates scientific validity rather than perceived impact and accepts interdisciplinary work. ([PLOS][6])                                                | Good if you want robustness, transparency and an honest confirm/disconfirm framing.                           |
| **Scientific Reports**                              | Suitable for a broad empirical modelling paper if results are strong and generalisable.                                                                                                                                                                                             | Good if you have clear cross-database replication.                                                            |
| **Scientific Data / Data in Brief**                 | Not ideal for the main empirical hypothesis paper, but good if you release a curated derivative dataset of RR/cognitive features plus code. Scientific Data publishes descriptions of scientifically valuable datasets. ([Nature][7])                                               |                                                                                                               |

My strongest recommendation: **Behavior Research Methods** first if the contribution is a method/pipeline; **Psychophysiology** first if the contribution is mind–body readiness-state theory and validation.

---

## B. Empirical study outline

### Core research question

Can public RR-interval and cognitive-control databases reveal reproducible latent state basins corresponding to the proposed Flow Zone / Trident-G categories?

For RR/ANS:

```text
BODY_READY
BODY_ACTIVATED_NEAR_ZONE
BODY_RIGID_PERSISTENT
BODY_ACTIVATED_RIGID
BODY_CHAOTIC_OVEREXCITED
```

For cognitive-control behaviour:

```text
MIND_READY
MIND_IN_ZONE
MIND_FLAT
MIND_LOCKED_IN
MIND_SPUN_OUT
```

Your cognitive spec already defines the cognitive states as lower Ψ-band, active Ψ-band, subcritical flat, supercritical locked-in and supercritical spun-out regimes.  The body spec defines a structurally parallel set of five autonomic states. 

---

# Study 1 — RR/ANS state-basin validation

## Suitable public RR / ECG datasets

| Dataset                                                            | Why use it                                                                                                                                                                                                                                             |
| ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Autonomic Aging — PhysioNet**                                    | Best large-scale RR/ECG discovery dataset. It includes ECG and continuous non-invasive blood pressure recordings from **1,121 healthy volunteers** at rest, designed to quantify cardiovascular autonomic function across ageing. ([physionet.org][8]) |
| **Normal Sinus Rhythm RR Interval Database — PhysioNet**           | Strong long-duration RR validation set: **54 long-term ECG recordings** with beat annotation files from subjects in normal sinus rhythm. ([physionet.org][9])                                                                                          |
| **MIT-BIH Normal Sinus Rhythm Database**                           | Smaller but useful raw ECG validation set: **18 long-term ECG recordings** from subjects with no significant arrhythmias. ([archive.physionet.org][10])                                                                                                |
| **Fantasia Database**                                              | Useful age/replication set: ECG and respiration recordings from **20 young and 20 elderly healthy subjects**, two hours each, resting sinus rhythm. ([physionet.org][11])                                                                              |
| **RR interval time series from healthy subjects — PhysioNet**      | Useful for broad healthy RR data across age ranges. It contains RR interval time series from healthy subjects aged from infancy to 55 years. ([physionet.org][12])                                                                                     |
| **WESAD**                                                          | Small but valuable labelled stress/affect validation set: **15 subjects**, chest ECG plus multimodal signals under stress/affect conditions. ([UCI Machine Learning Repository][13])                                                                   |
| **Wearable physiological signals under acute stress and exercise** | Useful external criterion dataset for stress/exercise labels; includes **n = 36** healthy participants and self-reported stress. ([Nature][14])                                                                                                        |

## Uploaded RR example

Your uploaded `002.txt` file contains **68,029 RR intervals**, giving about **15.9 hours** of data. With 300-beat non-overlapping windows, it yields **226 usable windows**; with 600-beat windows, **113 windows**; with 1500-beat windows, **45 windows**. Mean RR is about **840 ms**, corresponding to mean HR of about **71.4 bpm**. 

That is excellent for a **within-person state-profile example**, but not enough by itself for a population GMM. It can demonstrate windowing, feature extraction, state-transition logic and personal-baseline estimation.

## RR metrics to extract

Use exactly the body dynamics-core vector:

```text
hr
alpha1
lag1
lag2
roughness
sign_change
sd1_sd2
perm_entropy3
diff_entropy
```

The rationale is already clear in your spec:

| Metric          | Rationale                                                    |
| --------------- | ------------------------------------------------------------ |
| `hr`            | activation / mobilisation                                    |
| `alpha1`        | DFA short-term scaling / criticality-like temporal structure |
| `lag1`, `lag2`  | persistence / locking / attractor depth                      |
| `roughness`     | local beat-to-beat freedom relative to total variability     |
| `sign_change`   | alternating adaptability versus monotonic/persistent drift   |
| `sd1_sd2`       | Poincaré short/long variability balance                      |
| `perm_entropy3` | ordinal irregularity / local unpredictability                |
| `diff_entropy`  | diversity of physiological transitions                       |

These are the same features your body classifier defines for the RR model. 

## RR preprocessing

Use:

```text
valid_rr = 300–2000 ms
window_size = 300 valid intervals
step_size = 300 for discovery
step_size = 30–60 for live/smoothed analysis
```

Your spec already recommends 300 valid intervals as the minimum and 600–1500 as preferable for a single body check. 

---

# Study 2 — Cognitive-control behavioural state-basin validation

## Suitable public cognitive-control databases

| Dataset                                                     | Why use it                                                                                                                                                                                                                                                   |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **ACDC — Attentional Control Data Collection**              | Best first-line behavioural discovery source. It is an SQL database for attentional-control tasks and currently includes **64 datasets from 12 publications**, including Stroop, Simon and Flanker tasks. ([PMC][15])                                        |
| **COG-BCI**                                                 | Best cross-modal state dataset. It includes **29 participants**, **3 sessions**, four tasks — MATB, N-back, PVT and Flanker — and over 100 hours of EEG/ECG data plus behavioural and subjective measures. ([PMC][16])                                       |
| **DMCC / DMCC55B**                                          | Strong theory fit for proactive/reactive cognitive control. The DMCC55B dataset includes **55 healthy young adults** performing AX-CPT, cued task-switching, Sternberg working memory and Stroop, with self-report and behavioural data. ([WashU Sites][17]) |
| **OpenNeuro ds004636 self-regulation dataset**              | Broad cross-task validation. It includes **103 healthy adults** with cognitive tasks, surveys and neuroimaging measurements evaluating self-regulation. ([PMC][18])                                                                                          |
| **OpenNeuro Simon-conflict / other task-specific datasets** | Useful as external stress tests for conflict/switching generalisation; one Simon-conflict dataset includes **146 subjects** including Parkinson’s and control participants. ([OpenNeuro][19])                                                                |

## Cognitive metrics to extract

Use the cognitive dynamics-core vector:

```text
cog_rate
cog_alpha1
cog_lag1
cog_lag2
cog_roughness
cog_sign_change
cog_sd1_sd2
cog_perm_entropy3
cog_diff_entropy
switch_cost_rt
switch_cost_acc
perseveration
```

Your cognitive spec defines the rationale as activation/throughput, critical scaling, persistence/locking, local irregularity, entropy-like instability and task-relevant constraint. 

Key point: do **not** compute dynamics on raw RT alone. First fit an expected RT model and derive residuals:

```text
rt_ms ~ ratio + is_switch + soa_ms + trial_index + stimulus_dimension
```

Then compute state dynamics on `rt_residual_t`, `efficiency_t`, or the signed update series:

```text
u_t = z(efficiency_t) - z(rt_residual_t) + z(correct_t)
```

That step is essential because otherwise the GMM may simply learn “this was a switch trial” or “this was a hard condition” rather than a latent cognitive state. 

## Cognitive trial-count requirement

Your spec gives a practical division:

```text
Express:      40–50 trials  → aggregate features only
Core:         80–120 trials → preferred for dynamics-core features
Calibration: 150–250 trials → better personal model fitting
```

For DFA and entropy estimates, use **Core or Calibration** whenever possible. 

---

# C. Sample-size / participant-dataset calculation

There is no clean classical power calculation for unsupervised GMM discovery, so I would pre-register a **parameter-count + stability** rule.

For a full-covariance Gaussian mixture model:

```text
parameters = k × [p + p(p + 1)/2] + (k − 1)
```

where:

```text
p = number of features
k = number of mixture components
```

## RR model

For RR:

```text
p = 9 features
k = 5 product states
parameters = 274
```

For the exploratory internal model:

```text
p = 9
k = 10 internal GMM components
parameters = 549
```

A conservative unsupervised-modelling rule is:

```text
minimum windows = 5 × parameters
preferred windows = 10 × parameters
```

So:

| RR model                  | Parameters | Minimum windows | Preferred windows |
| ------------------------- | ---------: | --------------: | ----------------: |
| 5-component GMM           |        274 |          ~1,370 |            ~2,740 |
| 10-component internal GMM |        549 |          ~2,745 |            ~5,490 |

But windows within a person are not independent. Therefore, I would set the **participant-level target** as:

```text
RR discovery minimum: 300 participants
RR discovery preferred: 500–1,000+ participants
RR external validation: at least 2 independent datasets with ≥40–50 participants each
```

The **Autonomic Aging** dataset alone is large enough for discovery, because it has over 1,100 healthy volunteers. ([physionet.org][8]) Fantasia, MIT-BIH and Normal Sinus Rhythm RR are better as replication / sensitivity datasets, not sole discovery datasets.

For a new Flow Zone validation cohort, I would aim for:

```text
minimum: 100 participants × 10 sessions = 1,000 sessions/windows
better: 200 participants × 10–20 sessions = 2,000–4,000 sessions/windows
ideal: 300 participants × 20 sessions = 6,000 windows
```

That aligns with your body spec’s personal-baseline rule: use population GMM initially, but move to personal baselining after **10–20 sessions**, preferably **30+ sessions across contexts**. 

## Cognitive-control model

For cognition:

```text
p = 12 features
k = 5 product states
parameters = 454
```

For a richer internal model:

```text
p = 12
k = 10 internal components
parameters = 909
```

So:

| Cognitive model           | Parameters | Minimum windows | Preferred windows |
| ------------------------- | ---------: | --------------: | ----------------: |
| 5-component GMM           |        454 |          ~2,270 |            ~4,540 |
| 10-component internal GMM |        909 |          ~4,545 |            ~9,090 |

Here a “window” might be a participant × task × block segment with enough trials for dynamics features. Since cognitive windows are shorter and more task-confounded than RR windows, I would set the participant target higher:

```text
cognitive discovery minimum: 300 participants across datasets
cognitive preferred: 500–1,000 participants/windows-rich observations
external validation: at least 2 independent databases
```

ACDC is the best discovery source because it pools many attentional-control datasets. COG-BCI alone is too small for discovery, but excellent for physiological/subjective validation because it combines ECG, EEG, behavioural tasks and repeated sessions. ([PMC][15])

---

# D. Core analyses

## 1. Descriptive geometry

Run these first:

```text
feature distributions
missingness / artefact summaries
correlation heatmaps
PCA
exploratory factor analysis
UMAP / t-SNE visualisation
subject/dataset/task variance decomposition
```

Purpose: check whether the features behave as intended before fitting clusters.

Expected PCA axes:

| Axis                     | RR expression                   | Cognitive expression                 |
| ------------------------ | ------------------------------- | ------------------------------------ |
| Activation / throughput  | HR                              | cog_rate / efficiency                |
| Scaling / organisation   | DFA alpha1                      | cog_alpha1                           |
| Persistence / locking    | lag1, lag2                      | cog_lag1, cog_lag2, perseveration    |
| Local instability        | roughness, sign-change, SD1/SD2 | cog_roughness, sign-change, SD1/SD2  |
| Entropy-like fluctuation | perm_entropy3, diff_entropy     | cog_perm_entropy3, cog_diff_entropy  |
| Task constraint          | —                               | switch cost, accuracy, perseveration |

## 2. Primary unsupervised models

Use several models, not just Gaussian modelling.

| Model                         | Python library                            | Why use it                                                                              |
| ----------------------------- | ----------------------------------------- | --------------------------------------------------------------------------------------- |
| **Gaussian Mixture Model**    | `sklearn.mixture.GaussianMixture`         | Primary model. Gives posterior probabilities and supports BIC/AIC.                      |
| **Bayesian Gaussian Mixture** | `sklearn.mixture.BayesianGaussianMixture` | Tests whether the number of components collapses naturally or supports multiple basins. |
| **HDBSCAN**                   | `hdbscan`                                 | Density-based, does not require fixed k, can detect noise/outliers.                     |
| **Agglomerative clustering**  | `sklearn.cluster.AgglomerativeClustering` | Useful for dendrogram-like structure and sensitivity analysis.                          |
| **Spectral clustering**       | `sklearn.cluster.SpectralClustering`      | Tests non-elliptical cluster structure.                                                 |
| **k-means / k-medoids**       | `sklearn`, `sklearn_extra`                | Simple baseline; should be beaten by richer models if the theory is right.              |
| **Hidden Markov Model**       | `hmmlearn` or `pomegranate`               | Best for temporal smoothing and state-transition modelling.                             |
| **Mixed-effects models**      | `statsmodels`, `pymer4`                   | Tests whether state labels predict outcomes beyond subject, task and dataset effects.   |

## 3. Cluster validation

Use:

```text
BIC / AIC
integrated completed likelihood if available
silhouette score
Davies–Bouldin index
Calinski–Harabasz index
posterior entropy
minimum component size
bootstrap adjusted Rand index
bootstrap Jaccard stability
leave-one-dataset-out stability
leave-one-subject-out stability
```

The body spec already lists the key validation principles: cluster stability, within-person reliability, cross-device robustness, sensitivity to artefact cleaning, posterior calibration, prediction beyond simple baselines and comparison against HR/RMSSD-only baselines. 

## 4. Baseline models to beat

For RR:

```text
HR only
RMSSD only
HR + RMSSD
DFA alpha1 only
standard HRV readiness score
```

These are the explicit body-classifier baselines. 

For cognition:

```text
accuracy only
RT only
rate only
rate + RT CV
existing heuristic decision tree
task-condition labels only
```

Your cognitive spec defines rate-only, accuracy-only, rate + RT CV and the existing Flow-Zone-Mind heuristic tree as baseline models. 

## 5. Criterion and predictive validity

For RR:

```text
rest vs stress vs exercise labels
age effects
posture/context effects
self-reported stress where available
within-person state-transition plausibility
```

For cognition:

```text
next-block accuracy
next-block RT variability
lapse/miss rate
switch cost
post-error slowing
self-rated workload / focus / fatigue
task difficulty labels
condition labels: proactive/reactive, conflict, switching, PVT vigilance
```

The COG-BCI dataset is particularly useful here because it includes repeated sessions, multiple cognitive tasks and EEG/ECG data designed to elicit cognitive states. ([PMC][16])

## 6. Cross-modal validation

Where ECG and cognitive behaviour are available in the same dataset, especially COG-BCI:

```text
derive RR body-state labels
derive cognitive state labels
test alignment / dissociation
```

Use:

```text
mutual information
adjusted mutual information
Cramér’s V
canonical correlation analysis
partial least squares
multilevel logistic models
state-transition coupling
cross-lagged models where timing permits
```

Important: perfect agreement is **not** expected. Your spec explicitly states that mind and body classifiers should be structurally aligned, but disagreement is meaningful rather than noise. 

---

# E. Confirmatory criteria

The classification is supported if:

```text
1. A 4–5 product-state solution is stable across resampling.
2. A richer internal GMM, e.g. k = 8–10, can be cleanly merged into the five product states.
3. RR clusters replicate across at least two independent RR datasets.
4. Cognitive clusters replicate across at least two independent cognitive-control datasets.
5. The state labels predict outcomes better than simple HR/RMSSD or RT/accuracy models.
6. State labels have interpretable transition dynamics.
7. Personal baselines improve classification without destroying population-level topology.
8. Cognitive and body states show partial alignment, but also interpretable dissociation.
```

The model is weakened if:

```text
1. BIC/AIC consistently favour 1–2 undifferentiated components.
2. Clusters only reflect dataset identity, task type or trial condition.
3. Components are unstable under bootstrap or leave-dataset-out tests.
4. “Locked-in” and “spun-out” cannot be empirically separated.
5. Labels fail to predict outcomes beyond simple baselines.
6. A rate/accuracy-only or HR/RMSSD-only model performs as well as the dynamics-core model.
```

---

# F. Proposed manuscript structure

## Abstract

Briefly state that the project tests whether RR intervals and cognitive-control behaviour contain reproducible latent state basins corresponding to readiness, near-zone, rigid/subcritical, activated-rigid/locked-in and chaotic/spun-out regimes.

## Introduction

Cover:

```text
state-dependent performance
HRV/RR dynamics
cognitive control as information resolution under uncertainty
limitations of mean HRV and mean RT/accuracy
rationale for latent state-basin modelling
```

## Methods

### Study 1: RR/ANS data

Include:

```text
datasets
RR preprocessing
300-beat windows
feature extraction
GMM / Bayesian GMM / HDBSCAN / HMM
validation strategy
```

### Study 2: cognitive-control data

Include:

```text
datasets
trial-level cleaning
RT residualisation
u_t construction
feature extraction
cross-task residualisation
GMM / Bayesian GMM / HDBSCAN / HMM
validation strategy
```

### Optional Study 3: mind–body bridge

Use COG-BCI or any dataset with ECG + cognitive-task behaviour:

```text
body-state labels
cognitive-state labels
alignment / dissociation analysis
```

## Results

Expected tables:

```text
feature descriptive statistics
PCA loadings
GMM BIC/AIC by k
cluster profiles
cluster stability metrics
baseline comparison
external validation
state-transition matrices
mind–body alignment matrix
```

## Discussion

Discuss:

```text
whether the five-zone topology is supported
which states are robust
which states collapse or need revision
whether public datasets generalise to MFT-M
limits of “brain state” language
next step: dedicated Flow Zone / CCC calibration study
```

---

# G. Python execution stack

In VS Code, I would use:

```text
pandas, numpy, scipy
scikit-learn
statsmodels
neurokit2
wfdb
antropy
nolds
hdbscan
umap-learn
hmmlearn or pomegranate
pingouin
matplotlib
seaborn only for private exploration, not publication if you want plain matplotlib outputs
```

Core modelling objects:

```python
from sklearn.preprocessing import StandardScaler, RobustScaler
from sklearn.decomposition import PCA, FactorAnalysis
from sklearn.mixture import GaussianMixture, BayesianGaussianMixture
from sklearn.cluster import AgglomerativeClustering, SpectralClustering, KMeans
from sklearn.metrics import silhouette_score, adjusted_rand_score, adjusted_mutual_info_score
from sklearn.model_selection import GroupKFold, LeaveOneGroupOut
```

For RR:

```python
import wfdb
import neurokit2 as nk
import antropy as ant
```

For mixed-effects validation:

```python
import statsmodels.formula.api as smf
```

---

## Bottom line

The strongest empirical design is:

```text
Study 1:
Large RR databases → dynamics-core RR features → GMM/HDBSCAN/HMM → validate ANS zones.

Study 2:
Public cognitive-control databases → residualised behavioural dynamics → GMM/HDBSCAN/HMM → validate cognitive zones.

Study 3:
Datasets with ECG + cognition → test partial mind–body alignment and dissociation.
```

For discovery, I would aim for:

```text
RR:        300 minimum, 500–1,000+ preferred participants
Cognition: 300 minimum, 500–1,000 preferred participants/windows-rich observations
```

The RR side can probably be powered well using Autonomic Aging plus PhysioNet replication sets. The cognitive side is best powered by ACDC as the broad behavioural discovery set, with COG-BCI and DMCC as theoretically rich validation sets.

[1]: https://link.springer.com/journal/13428?utm_source=chatgpt.com "Behavior Research Methods | Springer Nature Link"
[2]: https://onlinelibrary.wiley.com/journal/14698986?utm_source=chatgpt.com "Psychophysiology"
[3]: https://www.sciencedirect.com/journal/international-journal-of-psychophysiology?utm_source=chatgpt.com "International Journal of Psychophysiology"
[4]: https://www.sciencedirect.com/journal/biological-psychology?utm_source=chatgpt.com "Biological Psychology | Journal"
[5]: https://link.springer.com/journal/41235/volumes-and-issues/6-1?page=2&utm_source=chatgpt.com "Volume 6, Issue 1 | Cognitive Research - Springer Nature"
[6]: https://journals.plos.org/plosone/s/journal-information?utm_source=chatgpt.com "Journal Information | PLOS One"
[7]: https://www.nature.com/sdata/?utm_source=chatgpt.com "Scientific Data"
[8]: https://physionet.org/content/autonomic-aging-cardiovascular/?utm_source=chatgpt.com "Autonomic Aging: A dataset to quantify changes of ..."
[9]: https://physionet.org/content/nsr2db/?utm_source=chatgpt.com "Normal Sinus Rhythm RR Interval Database v1.0.0"
[10]: https://archive.physionet.org/physiobank/database/nsrdb/?utm_source=chatgpt.com "The MIT-BIH Normal Sinus Rhythm Database"
[11]: https://physionet.org/content/fantasia/?utm_source=chatgpt.com "Fantasia Database v1.0.0"
[12]: https://physionet.org/content/rr-interval-healthy-subjects/?utm_source=chatgpt.com "RR interval time series from healthy subjects v1.0.0"
[13]: https://archive.ics.uci.edu/ml/datasets/WESAD%2B%28Wearable%2BStress%2Band%2BAffect%2BDetection%29?utm_source=chatgpt.com "WESAD (Wearable Stress and Affect Detection)"
[14]: https://www.nature.com/articles/s41597-025-04845-9?utm_source=chatgpt.com "Wearable Physiological Signals under Acute Stress and ..."
[15]: https://pmc.ncbi.nlm.nih.gov/articles/PMC12187800/?utm_source=chatgpt.com "Attentional control data collection: A resource for efficient data ..."
[16]: https://pmc.ncbi.nlm.nih.gov/articles/PMC9918545/?utm_source=chatgpt.com "Open multi-session and multi-task EEG cognitive Dataset for ..."
[17]: https://sites.wustl.edu/dualmechanisms/released-data-sets/?utm_source=chatgpt.com "Released Data Sets | Dual Mechanisms of Cognitive Control"
[18]: https://pmc.ncbi.nlm.nih.gov/articles/PMC11271451/?utm_source=chatgpt.com "Cognitive tasks, anatomical MRI, and functional MRI data ..."
[19]: https://openneuro.org/datasets/ds004580/versions/1.0.0?utm_source=chatgpt.com "Simon-conflict Task."
