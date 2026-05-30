# Trident-G Zone Validation Protocol v0.2

## Implementation-ready protocol for body-state and cognitive-state discovery

**Purpose:** validate whether Trident-G / Flow Zone body and cognitive zones can be recovered as reproducible dynamical regimes from public RR-interval and cognitive-control datasets.

**Status:** implementation draft  
**Scope:** public-data discovery and validation study; not yet a production classifier specification  
**Primary outputs:** feature matrices, unsupervised state-discovery reports, factor structure, temporal dynamics, cross-domain coupling estimates, and decision rules for whether zone labels should be categorical or continuous-region based.

---

# 1. Core Study Rationale

Trident-G predicts that adaptive performance depends on whether the organism is operating inside, below, or above a near-critical Ψ-band. The validation study asks whether this state-space can be detected empirically in two parallel data streams:

1. **Body / ANS stream:** RR or NN interval dynamics.
2. **Cognitive stream:** trial-level behavioural dynamics from cognitive-control tasks.

The study does **not** assume that the proposed zones are already proven. It tests whether public datasets support a small number of reproducible dynamical regimes corresponding to:

## 1.1 Body states

| Product state | Trident-G interpretation | Practical meaning |
|---|---|---|
| `BODY_READY` | Lower Ψ-band / access state | Flexible, available physiology. Good candidate for warm-up, work entry, or normal training. |
| `BODY_ACTIVATED_NEAR_ZONE` | Upper Ψ-band / active near-critical physiology | Mobilised but still organised. Performance possible, with recovery monitoring. |
| `BODY_RIGID_PERSISTENT` | Subcritical / under-mobile / autopilot basin | Organised but persistent. May support familiar routines, but poor for flexible adaptation. |
| `BODY_ACTIVATED_RIGID` | Supercritical locked-in / constraint-dominant fork | Mobilised and over-constrained. Potentially forceful but brittle and costly. |
| `BODY_CHAOTIC_OVEREXCITED` | Supercritical spun-out / entropy-dominant fork | Highly activated but poorly organised. Avoid strong performance claims. |

## 1.2 Cognitive states

| Product state | Trident-G interpretation | Practical meaning |
|---|---|---|
| `MIND_READY` | Lower Ψ-band / access state | Available but not fully engaged. Warm-up may be useful. |
| `MIND_IN_ZONE` | Active Ψ-band / near-critical cognition | Good balance of throughput, flexibility and constraint. |
| `MIND_FLAT` | Subcritical / under-activated | Low throughput and weak reconfiguration. |
| `MIND_LOCKED_IN` | Supercritical locked-in / constraint-dominant fork | High persistence, switch cost or perseveration. Narrow productivity possible; poor reconfiguration. |
| `MIND_SPUN_OUT` | Supercritical spun-out / entropy-dominant fork | High irregularity, unstable performance and weak task-set constraint. |

The study should treat these as **candidate product-level labels**, not as fixed ground-truth categories. The empirical question is whether the data support these zones, a smaller number of broader zones, a larger number of internal subtypes, or a continuous state-space rather than categorical clusters.

---

# 2. Study Design Overview

The validation study has four layers.

| Layer | Purpose | Main question |
|---|---|---|
| **Study 1: Body-state discovery** | Extract RR dynamics and discover body-state basins | Do RR dynamics form reproducible states consistent with Trident-G body zones? |
| **Study 2: Cognitive-state discovery** | Extract residualised trial-level cognitive dynamics and discover cognitive basins | Do cognitive-control behaviours form reproducible states consistent with Trident-G mind zones? |
| **Study 3: Validation and prediction** | Test whether discovered states predict external labels and outcomes | Do zone labels predict stress, task demand, fatigue or performance better than simple baselines? |
| **Study 4: Cross-domain coupling** | Test partial alignment between body and cognitive states where simultaneous data exist | Are body and mind states partially coupled, while allowing meaningful dissociation? |

---

# 3. Dataset Strategy

## 3.1 Body / RR datasets

### Tier 1: launch datasets

| Dataset | Role | Expected use |
|---|---|---|
| **LEMON peripheral physiology** | Resting lower-cusp / baseline topology | Use PPG, beat-to-beat BP or clean physiology streams. Treat as resting-state baseline, not full-zone validation. |
| **WESAD** | Stress-state and upper-cusp validation | Use chest ECG during baseline, stress and amusement conditions. Essential for testing high-activation states. |
| **Autonomic Aging / large PhysioNet RR resources** | Population robustness and age-related variance | Use as large-sample discovery or replication source where RR/ECG quality permits. |
| **Normal Sinus Rhythm / Fantasia / related PhysioNet datasets** | Replication and sensitivity testing | Use for robustness across long recordings, age groups and recording conditions. |

### Tier 2: additional datasets

| Dataset | Role | Expected use |
|---|---|---|
| **MIT Driver Stress** | Ecological stress validation | Tests whether body-zone topology generalises beyond quiet laboratory rest. |
| **SWELL / workplace stress datasets** | Real-world work-state validation | Tests work-like load, stress and fatigue states. |
| **Own Polar H10 / app data** | Product calibration | Final thresholding and personal-baseline calibration for Flow Zone. |

## 3.2 Cognitive-control datasets

The cognitive side of the study should distinguish **direct MFT-M analogues**, **mechanistic control analogues**, and **broader cognitive-state validation tasks**. The shared feature-extraction layer is valid only after task-design effects have been residualised and task-specific control costs have been coded explicitly.

### Tier 1: launch datasets

| Dataset | Role | Expected use | MFT-M comparability |
|---|---|---|---|
| **ACDC: Attentional Control Data Collection** | Broad attentional-control discovery | Stroop, Simon, Flanker and related tasks. Use for conflict-control dynamics, residual RT, accuracy, post-error effects and latent state discovery. | **High partial analogue**: strong for conflict and task-set constraint; weaker for explicit switch/repeat structure unless task-switching subsets are present. |
| **DMCC** | Theory-driven control validation | AX-CPT, cued task-switching, Sternberg and Stroop. Use for proactive/reactive control, task switching, interference and working-memory-control validation. | **Very high for cued task-switching**, high for Stroop, moderate for AX-CPT/Sternberg. |
| **COG-BCI** | Multimodal bridge dataset | MATB, N-back, PVT and Flanker with EEG/ECG/subjective validation. Use mainly for body–mind coupling and cognitive-state validation. | **High partial analogue for Flanker**; moderate for N-back/MATB; low-to-moderate for PVT. |
| **Lumosity OSF task-switching datasets** | Large-scale task-switching, learning and transfer-dynamics validation | Use for task-switching practice, relearning, switch-cost dynamics, wrapper sensitivity and large-N behavioural modelling. | **Very high behavioural analogue** for switching and rule-change dynamics, but with online/gameplay confounds. |
| **OpenNeuro self-regulation / executive-control datasets** | Cross-task validation | Use for generalisation across inhibition, attention, task-switching, working memory and self-regulation tasks. | Variable; classify task-by-task. |

### 3.2.1 Cognitive task comparability taxonomy

Use the following taxonomy when importing cognitive datasets.

| Task family | Examples | Protocol role | MFT-M comparability |
|---|---|---|---|
| **Direct task-set switching** | DMCC cued task-switching, Lumosity Ebb and Flow, Gabor task-switching where accessible | Primary behavioural analogue | **Very high** |
| **Conflict-control tasks** | Flanker, Stroop, Simon | Mechanistic analogue for competing information and task-relevant constraint | **High partial** |
| **Context / proactive-control tasks** | AX-CPT, DPX | Validation of control mode and context maintenance | **Moderate** |
| **Working-memory updating** | N-back, Sternberg | Load, updating and controlled retrieval validation | **Moderate** |
| **Vigilance / lapse tasks** | PVT, SART | Flatness, fatigue and lapse validation | **Low-to-moderate** |
| **Ecological workload tasks** | MATB and related multitask batteries | Workload and multitasking-state validation | **Moderate** |
| **MFT-M / CCC itself** | Gabor majority decision with feature switching | Target task for final calibration | **Gold-standard calibration task** |

### 3.2.2 Implication for analysis

Do not treat all cognitive-control tasks as direct equivalents of MFT-M. Instead:

1. Use **task-switching datasets** as the closest public analogue to CCC/MFT-M.
2. Use **Flanker, Stroop and Simon** as conflict-control analogues.
3. Use **AX-CPT/DPX, N-back, PVT, SART, Sternberg and MATB** as broader cognitive-state validation tasks.
4. Use MFT-M / CCC data itself for final thresholding and product calibration.

## 3.3 The resting-cusp problem

Resting datasets mainly sample the lower and central parts of the Trident-G cusp. They may reveal `BODY_READY`, `BODY_RIGID_PERSISTENT` and perhaps lower near-zone states, but they may not adequately sample high-activation locked-in or spun-out states.

Therefore:

- Failure to recover all five zones from rest-only data is **not** a theory failure.
- Stress, workload, exercise or cognitive-demand datasets are required to test upper-cusp states.
- The correct interpretation of rest-only findings is: **lower-cusp topology**, not full-zone topology.

---

# 4. Body Feature Extraction

## 4.1 Input

Each body recording should provide a continuous RR or NN interval series:

```text
rr_ms = [interval_1_ms, interval_2_ms, ...]
```

Preferred input sources:

- ECG-derived R-peaks;
- high-quality chest-strap RR intervals;
- clean PPG or beat-to-beat BP-derived intervals when ECG is unavailable.

Avoid treating heart-rate-only streams as RR interval data.

## 4.2 Preprocessing

Apply the following initial cleaning rules:

```text
valid_rr = 300 ms <= rr_ms <= 2000 ms
```

Additional cleaning:

1. Remove non-numeric values.
2. Remove physiologically impossible intervals.
3. Remove intervals deviating more than 20% from a short local median unless there is a dataset-specific reason not to.
4. Interpolate isolated artefacts only when the invalid segment is short.
5. Reject or flag windows with more than 5% corrected or missing intervals.
6. Retain artefact percentage and correction count for every window.

## 4.3 Windowing

For discovery:

```text
window_size = 300 valid intervals
step_size = 300 intervals
```

For temporal smoothing and app-like analysis:

```text
window_size = 300 valid intervals
step_size = 30–60 intervals
```

Overlapping windows must not be treated as independent samples in inferential statistics.

## 4.4 Primary body metrics

Compute the following 9 metrics for every valid window.

| Metric | Symbol | Definition | Trident-G interpretation |
|---|---|---|---|
| Activation | `hr` | `60000 / mean(rr_ms)` | Mobilisation / activation / ξ-related load. |
| Scaling | `alpha1` | Short-scale DFA exponent over RR intervals | Criticality-like temporal organisation. Meaning depends on the multivariate pattern. |
| Persistence | `lag1` | Lag-1 autocorrelation of RR | Oscillatory memory / persistence. Can be healthy or locking depending on entropy and roughness. |
| Persistence | `lag2` | Lag-2 autocorrelation of RR | Slower persistence / oscillatory carryover. |
| Local freedom | `roughness` | RMSSD or MASD normalised by RR SD | Beat-to-beat freedom relative to total variability. |
| Alternation | `sign_change` | Proportion of reversals in successive RR differences | Local alternation; low values indicate drift/persistence, high values may indicate instability. |
| Poincaré balance | `sd1_sd2` | SD1/SD2 ratio | Short-term scatter relative to longer-term structure. |
| Ordinal entropy | `perm_entropy3` | Normalised permutation entropy, order 3 | Local ordinal unpredictability. |
| Transition entropy | `diff_entropy` | Entropy of successive RR changes, not raw RR distribution | Diversity of physiological transitions. |

## 4.5 Interpretation rule

No single metric should define a body state. In particular:

- `alpha1` should not classify zones by itself.
- `lag1` is not automatically “locking”. In HRV it may reflect healthy oscillatory structure.
- High persistence becomes “rigidity” only when paired with low roughness, low entropy and poor state flexibility.
- Low scaling becomes “fragmentation” mainly when paired with high activation, high roughness and high entropy.

State interpretation must be based on multivariate feature patterns.

---

# 5. Cognitive Feature Extraction

## 5.1 Input

Each cognitive dataset should provide trial-level records with as many of the following fields as possible:

| Field | Purpose |
|---|---|
| `subject_id` | Personal calibration and grouping. |
| `session_id` | Session-level state modelling. |
| `trial_index` | Time-series ordering. |
| `task` | Dataset/task identity. |
| `condition` | Trial condition, e.g. congruent/incongruent, switch/repeat. |
| `ratio` | Task difficulty or evidence ratio where applicable. |
| `is_switch` | Whether the task-set or response dimension switched. |
| `soa_ms` | Stimulus onset asynchrony or equivalent timing variable. |
| `stimulus_dimension` | Relevant task feature dimension. |
| `correct_response` | Expected response. |
| `response` | Actual response. |
| `correct` | Binary accuracy. |
| `rt_ms` | Reaction time. |
| `miss` | No response / lapse. |
| `early_response` | Response before valid window. |
| `device_timing_flags` | Browser, timing, focus or frame flags where available. |

## 5.2 Minimum trial counts

| Mode | Trials | Use |
|---|---:|---|
| Express | 40–50 | Aggregate features only; avoid strong entropy/DFA claims. |
| Core | 80–120 | Preferred minimum for dynamics-core features. |
| Calibration | 150–250 | Best for personal modelling and stable dynamic estimates. |

Datasets or subjects below the Express threshold may be used for basic speed/accuracy analyses but not for full zone discovery.

## 5.3 Residualisation

Do not compute cognitive dynamics on raw RT alone. First remove known design effects.

Fit an expected RT model within subject where possible:

```text
rt_ms ~ task + condition + difficulty + control_cost_type + is_switch + congruency + soa_ms + trial_index + stimulus_dimension
```

Where datasets differ, use the closest available design variables.

Examples:

```text
MFT-M / task-switching:     include switch/repeat, stimulus dimension, difficulty ratio, trial index
Flanker / Stroop / Simon:  include congruency/interference condition, trial index, response mapping
AX-CPT / DPX:              include cue-probe condition and proactive/reactive control contrast
N-back / Sternberg:        include load, lure status, trial index and block
PVT / SART:                include trial index, foreperiod, lapse status and block
MATB:                      include task stream, workload condition and event type
```

Then compute:

```text
rt_residual_t = observed_rt_t − expected_rt_t
```

This step is essential. Without it, clustering may recover task difficulty or trial condition rather than cognitive state.

## 5.4 Dynamic trial series

Construct a unified trial-level update series:

```text
u_t = z(efficiency_t) − z(rt_residual_t) + z(correct_t)
```

where:

```text
efficiency_t = correct_t / rt_seconds_t
```

or, if trials are binary information decisions:

```text
efficiency_t = correct_t × bits_t / rt_seconds_t
```

For simple binary decisions:

```text
bits_t = 1
```

For graded task difficulty, optionally weight trials using estimated information load.

## 5.5 Primary cognitive metrics

Compute the following metrics per subject × session × task block or valid trial window.

| Metric | Symbol | Definition | Trident-G interpretation |
|---|---|---|---|
| Throughput | `cog_rate` | Mean or median efficiency / `1/rt_ms` | Cognitive mobilisation / processing speed. |
| Scaling | `cog_alpha1` | Short-scale DFA exponent on `u_t` or residual series | Criticality-like cognitive update structure. |
| Persistence | `cog_lag1` | Lag-1 autocorrelation of `u_t` | Attractor depth / carryover / possible perseveration. |
| Persistence | `cog_lag2` | Lag-2 autocorrelation of `u_t` | Slower cognitive drift. |
| Local freedom | `cog_roughness` | RMSSD or MASD of `u_t` normalised by SD | Trial-to-trial flexibility or instability. |
| Alternation | `cog_sign_change` | Reversals in successive `u_t` differences | Local switching / alternation. |
| Poincaré balance | `cog_sd1_sd2` | SD1/SD2 of `u_t` | Short-term versus long-term variability. |
| Ordinal entropy | `cog_perm_entropy3` | Normalised permutation entropy, order 3 | Unpredictability of cognitive transitions. |
| Transition entropy | `cog_diff_entropy` | Entropy of successive `u_t` changes | Diversity of cognitive updates. |
| Control cost RT | `control_cost_rt` | RT cost for the task’s relevant control contrast | Reconfiguration, interference, context or workload cost. |
| Control cost accuracy | `control_cost_acc` | Accuracy cost for the task’s relevant control contrast | Accuracy cost of control demand. |
| Control cost type | `control_cost_type` | `switch`, `conflict`, `interference`, `proactive_context`, `working_memory_load`, `vigilance`, or `workload` | Indicates how the cost should be interpreted. |
| Perseveration | `perseveration` | Error-after-error or repeat-error tendency | Difficulty disengaging from prior state. |
| Post-error slowing | `post_error_slowing` | RT after error − RT after correct trial | Recovery / reconfiguration after error. |
| Lapse rate | `lapse_rate` | Missed or excessively slow responses, task-defined | Fatigue, disengagement or flatness marker. |

### 5.5.1 Task-specific control-cost definitions

Use a general `control_cost_*` field rather than forcing every dataset into a switch-cost model.

| Task family | `control_cost_type` | Example computation |
|---|---|---|
| MFT-M / CCC | `switch` | `mean(RT_switch) − mean(RT_repeat)`; `accuracy_repeat − accuracy_switch` |
| Cued task-switching | `switch` | `mean(RT_switch) − mean(RT_repeat)` |
| Flanker | `conflict` | `mean(RT_incongruent) − mean(RT_congruent)` |
| Stroop | `interference` | `mean(RT_incongruent) − mean(RT_congruent)` |
| Simon | `interference` | `mean(RT_noncorresponding) − mean(RT_corresponding)` |
| AX-CPT / DPX | `proactive_context` | Cost or error contrast for high-control cue-probe conditions |
| N-back / Sternberg | `working_memory_load` | High-load − low-load RT/accuracy cost |
| PVT / SART | `vigilance` | Lapse rate, RT variability and time-on-task cost rather than switch cost |
| MATB | `workload` | High-workload − low-workload performance or RT cost |

## 5.6 Cognitive interpretation rule

No single cognitive metric defines a zone.

- High `cog_lag1` may reflect useful stability or perseverative locking depending on switch cost, accuracy and entropy.
- High entropy may reflect adaptive exploration or spun-out instability depending on accuracy and throughput.
- Low switch cost may reflect flexibility, but if paired with low accuracy it may indicate weak task-set constraint rather than good control.

State interpretation must use multivariate profiles and external validation.

---

# 6. Discovery Analysis

## 6.1 Feature matrices

Create separate feature matrices:

```text
body_features.csv
cognitive_features.csv
```

Minimum columns:

```text
subject_id
dataset
session_id
condition
window_id
feature columns
artefact / timing quality fields
```

For body:

```text
hr, alpha1, lag1, lag2, roughness, sign_change, sd1_sd2, perm_entropy3, diff_entropy
```

For cognition:

```text
cog_rate, cog_alpha1, cog_lag1, cog_lag2, cog_roughness, cog_sign_change,
cog_sd1_sd2, cog_perm_entropy3, cog_diff_entropy,
control_cost_rt, control_cost_acc, control_cost_type,
perseveration, post_error_slowing, lapse_rate
```

## 6.2 Scaling

Use robust scaling or standard scaling after outlier review.

Recommended default:

```text
1. Inspect distributions.
2. Winsorise only if justified and documented.
3. Standardise features within dataset for discovery.
4. Repeat sensitivity analysis using pooled scaling.
5. For personal baselines, compute robust within-person z-scores after sufficient data exist.
```

## 6.3 PCA / factor geometry

Run PCA and, where sample size permits, exploratory factor analysis with oblique rotation.

Expected broad axes:

| Axis | Body expression | Cognitive expression |
|---|---|---|
| Activation / throughput | `hr` | `cog_rate`, efficiency |
| Scaling / organisation | `alpha1`, entropy metrics | `cog_alpha1`, cognitive entropy metrics |
| Persistence / locking | `lag1`, `lag2` | `cog_lag1`, `cog_lag2`, perseveration |
| Local flexibility / instability | `roughness`, `sign_change`, `sd1_sd2` | `cog_roughness`, `cog_sign_change`, `cog_sd1_sd2` |
| Task constraint | — | switch costs, accuracy, perseveration |

PCA is not the final classifier. It is used to inspect whether the feature space has interpretable geometry.

## 6.4 Unsupervised discovery models

Run multiple discovery models.

| Model | Purpose |
|---|---|
| Gaussian Mixture Model, `k = 1–10` | Primary probabilistic basin model; use BIC/AIC and posterior confidence. |
| Bayesian Gaussian Mixture | Tests whether extra components collapse or remain active. |
| HDBSCAN | Tests whether density modes exist without imposing `k`. |
| Agglomerative clustering | Tests hierarchical structure. |
| k-means / k-medoids | Simple baseline clustering. |
| UMAP / t-SNE | Visualisation only; not confirmatory. |

## 6.5 Internal components versus product states

Do not require the best statistical model to have exactly four or five components.

Acceptable outcomes include:

```text
3 broad components
4–5 product-level components
6–10 internal components that merge cleanly into 4–5 product states
continuous manifold with weak categorical separability
```

A `k > 5` GMM is not a failure if the additional components are interpretable subtypes that merge into the Trident-G product zones.

---

# 7. Validation Analyses

## 7.1 Cluster stability

For each body and cognitive model, report:

```text
BIC / AIC curves
posterior entropy
minimum component size
silhouette score
Davies–Bouldin index
Calinski–Harabasz index
bootstrap adjusted Rand index
bootstrap Jaccard stability
leave-subject-out stability
leave-dataset-out stability
```

## 7.2 Baseline models to beat

### Body baselines

The zone model should be compared against:

```text
HR only
RMSSD only
HR + RMSSD
DFA alpha1 only
simple HRV readiness score where available
```

### Cognitive baselines

The cognitive zone model should be compared against:

```text
accuracy only
RT only
rate only
rate + RT CV
control-cost only
post-error slowing only
lapse-rate only
existing heuristic decision tree, if available
task-condition labels only
```

For task families without true switch/repeat structure, use the appropriate control-cost baseline rather than `switch_cost`.

A zone model is not strongly supported unless it predicts validation outcomes beyond these simpler alternatives.

## 7.3 External criterion validation

### Body criteria

Use available labels such as:

```text
baseline versus stress versus amusement
rest versus task versus recovery
rest versus city driving versus highway driving
subjective stress
exercise / workload condition
age or health group where appropriate
```

Expected validation pattern:

- Stress and high-load states should increase high-activation basin occupancy.
- Resting datasets should mainly sample lower-cusp and central states.
- High-activation rigid and chaotic states should be more common in stress/load datasets than quiet rest datasets.

### Cognitive criteria

Use available outcomes such as:

```text
next-block accuracy
next-block RT variability
lapse / miss rate
switch cost
post-error slowing
self-rated workload / focus / fatigue
condition-level control demand
learning / relearning performance
transfer to changed task wrappers
```

Expected validation pattern:

- `MIND_IN_ZONE` should predict better next-block efficiency and lower instability.
- `MIND_LOCKED_IN` should predict higher switch costs, perseveration or brittle performance under changed conditions.
- `MIND_SPUN_OUT` should predict instability, lapses, weak accuracy or poor convergence.
- `MIND_FLAT` should predict low throughput or weak engagement.

## 7.4 Mixed-effects validation

Use mixed-effects models to ensure that state labels are not merely dataset or subject artefacts.

Example body model:

```text
outcome ~ state + condition + dataset + artefact_pct + (1 | subject_id)
```

Example cognitive model:

```text
next_block_performance ~ state + task + condition + trial_count + (1 | subject_id) + (1 | dataset)
```

Also run variance decomposition for each feature:

```text
feature ~ 1 + (1 | subject_id) + (1 | dataset) + (1 | condition)
```

The protocol should report how much variance is within-person, between-person, between-condition and between-dataset.

---

# 8. Temporal-State Analysis

Where each subject has multiple windows or sessions, analyse state transitions.

## 8.1 Transition matrices

For body and cognition separately:

```text
state_t → state_t+1
```

Report:

```text
transition probability matrix
state dwell time
state recurrence
state entropy
state volatility
```

## 8.2 HMM smoothing

Use Hidden Markov Models or simple persistence smoothing to test whether state sequences have lawful temporal structure.

Questions:

```text
Are transitions non-random?
Are some transitions rare or asymmetric?
Do stress states show longer dwell times in high-activation zones?
Do rest/recovery states return toward ready/near-zone states?
```

For overlapping windows, temporal analysis is useful, but inferential tests must account for dependence.

---

# 9. Cross-Domain Body–Mind Coupling

Use only datasets containing simultaneous or near-simultaneous physiology and cognitive task behaviour.

## 9.1 Label mapping before coupling tests

Because unsupervised cluster labels are arbitrary, do not compute coupling directly on raw cluster numbers.

Required sequence:

```text
1. Cluster body features.
2. Interpret body clusters using feature profiles and condition labels.
3. Map body clusters to BODY_* labels or broader body regimes.
4. Cluster cognitive features.
5. Interpret cognitive clusters using feature profiles and behavioural outcomes.
6. Map cognitive clusters to MIND_* labels or broader cognitive regimes.
7. Test body–mind coupling using mapped labels.
```

## 9.2 Coupling metrics

Report:

```text
Cramér’s V
adjusted mutual information
Cohen’s κ after theory-label mapping
state-pair enrichment ratios
cross-lagged prediction where timing permits
body-state → next cognitive-state prediction
cognitive-state → next body-state prediction
```

Trident-G predicts partial coupling, not perfect agreement.

Expected pattern:

```text
BODY_READY + MIND_READY
BODY_ACTIVATED_NEAR_ZONE + MIND_IN_ZONE
BODY_ACTIVATED_RIGID + MIND_LOCKED_IN
BODY_CHAOTIC_OVEREXCITED + MIND_SPUN_OUT
```

But meaningful dissociations should also occur:

```text
MIND_IN_ZONE + BODY_ACTIVATED_RIGID = effortful output with recovery debt
MIND_IN_ZONE + BODY_CHAOTIC_OVEREXCITED = unstable high-performance edge
MIND_FLAT + BODY_READY = body available, mind needs warm-up
MIND_LOCKED_IN + BODY_READY = cognitive rigidity without body overload
```

Dissociation is therefore a result, not merely noise.

---

# 10. Decision Rules

## 10.1 Strong support

The zone model is strongly supported if:

```text
1. Discovery models identify 4–5 interpretable product states, or 6–10 internal components that merge cleanly into 4–5 product states.
2. Cluster solutions are stable under bootstrap and leave-dataset-out validation.
3. State labels predict external criteria beyond simple baselines.
4. Temporal transitions are non-random and interpretable.
5. Rest-only data show lower-cusp limitation, while stress/load data add upper-cusp states.
6. Body and cognitive states show partial, interpretable coupling where simultaneous data exist.
```

## 10.2 Moderate support

The model receives moderate support if:

```text
1. Resting body data show only 2–3 states, but stress/load datasets add high-activation basins.
2. Cognitive datasets show throughput, rigidity and instability axes, but only 3–4 broad states.
3. State labels predict some outcomes beyond baselines, but not consistently across all datasets.
4. Internal components require some revision of the product labels.
```

## 10.3 Continuous-state alternative

If PCA/UMAP/factor analysis reveals a smooth activation × complexity × persistence manifold, but categorical clustering is unstable, the correct conclusion is not necessarily theory failure.

Instead, report:

```text
Trident-G zones are better represented as regions of a continuous dynamical state space rather than discrete natural kinds.
```

In this case, product states should be implemented as probabilistic regions, not hard classes.

## 10.4 Weak support / failure

The zone model is weakened if:

```text
1. Features form a single Gaussian-like blob across discovery and validation datasets.
2. Clusters are unstable across bootstrap and leave-dataset-out validation.
3. Clusters map mainly to dataset identity, task type, artefact rate or preprocessing decisions.
4. State labels fail to predict outcomes beyond HR/RMSSD or RT/accuracy baselines.
5. Locked-in and spun-out branches cannot be separated even under stress/load conditions.
6. More than 80% of meaningful variance is between subjects or datasets, with little within-person state variability.
```

---

# 11. Milestone Plan

## Milestone 0 — Dataset acquisition and data dictionary

**Goal:** confirm dataset availability, file formats, participant counts and usable channels.

Deliverables:

```text
dataset_inventory.csv
data_dictionary_body.md
data_dictionary_cognition.md
preprocessing_decisions.md
```

## Milestone 1 — Pilot extraction

**Goal:** run extraction on a small subset to catch pipeline failures.

Body pilot:

```text
10 LEMON subjects
all WESAD subjects if feasible
300-beat RR windows
9 body features
artefact summary
```

Cognitive pilot:

```text
10–20 subjects from one cognitive dataset
trial residualisation
u_t construction
12 cognitive features where trial counts permit
```

Deliverables:

```text
pilot_body_features.csv
pilot_cognitive_features.csv
feature_distribution_report.md
artefact_report.md
```

## Milestone 2 — Discovery analysis

**Goal:** identify candidate structure in body and cognitive feature spaces.

Analyses:

```text
PCA / factor analysis
GMM k = 1–10
Bayesian GMM
HDBSCAN
UMAP visualisation
baseline comparisons
```

Deliverables:

```text
body_discovery_report.md
cognitive_discovery_report.md
bic_aic_curves.png
cluster_profile_tables.csv
pca_loadings.csv
```

## Milestone 3 — Validation analysis

**Goal:** test whether clusters predict labels and outcomes beyond simpler models.

Analyses:

```text
condition enrichment
mixed-effects outcome prediction
bootstrap cluster stability
leave-dataset-out validation
baseline model comparison
```

Deliverables:

```text
validation_report.md
baseline_comparison_table.csv
cluster_stability_table.csv
external_criterion_results.csv
```

## Milestone 4 — Temporal and cross-domain analysis

**Goal:** test transitions and body–mind coupling.

Analyses:

```text
state-transition matrices
HMM smoothing where appropriate
body–mind contingency
adjusted mutual information
state-pair enrichment
divergence profiles
```

Deliverables:

```text
temporal_state_report.md
mind_body_coupling_report.md
transition_matrices.csv
divergence_profile_table.csv
```

## Milestone 5 — Product translation

**Goal:** decide how results should inform Flow Zone classification.

Possible outputs:

```text
hard class model
probabilistic region model
continuous manifold model
hybrid model: broad product states + internal subtypes
```

Deliverables:

```text
flow_zone_classifier_recommendation.md
claim_safety_statement.md
next_validation_study_design.md
```

---

# 12. Implementation Deliverables

At the end of the first full analysis cycle, the project should contain:

| Deliverable | Description |
|---|---|
| `dataset_inventory.csv` | Dataset, N, channels, task types, labels, permissions and usable fields. |
| `body_features.csv` | Window-level RR features with quality metadata. |
| `cognitive_features.csv` | Subject/session/window-level cognitive features. |
| `cluster_models/` | Saved model objects and configuration files. |
| `discovery_report.md` | PCA, GMM, Bayesian GMM, HDBSCAN and visual topology. |
| `validation_report.md` | Baseline comparisons, external labels and mixed-effects results. |
| `temporal_report.md` | Transition matrices and HMM results. |
| `mind_body_report.md` | Coupling and dissociation analysis. |
| `claim_safety_statement.md` | What can and cannot be claimed from the validation study. |

---

# 13. Claim Safety

Allowed early research claim:

```text
The study tests whether RR-interval dynamics and cognitive-control behaviour contain reproducible dynamical state patterns consistent with Trident-G zone predictions.
```

Allowed if validation is positive:

```text
The results support a probabilistic state-space model in which body and cognitive readiness vary across interpretable regimes of activation, organisation, persistence and instability.
```

Avoid:

```text
The model proves brain criticality.
The app directly measures brain states.
The app diagnoses nervous system disorders.
The app proves flow state.
The zones are fixed biological types.
```

Preferred wording:

```text
criticality-like dynamics
readiness-state basins
probabilistic state estimates
mind–body readiness patterns
near-zone / off-zone classification
```

---

# 14. Immediate Next Steps

1. Finalise the dataset inventory and confirm accessible files.
2. Decide whether the first body launch set is:
   - LEMON + WESAD, or
   - Autonomic Aging + WESAD.
3. Decide whether the first cognitive launch set is:
   - ACDC + DMCC, or
   - Lumosity + ACDC.
4. Run pilot extraction on a small subset before full download/processing.
5. Freeze feature definitions before running discovery models.
6. Pre-register the decision rules if the study is intended for publication.

---

# 15. Recommended First Implementation Route

For the first implementation cycle, use the smallest dataset combination that tests both lower-cusp and upper-cusp assumptions:

## Body

```text
LEMON or Autonomic Aging = baseline / lower-cusp structure
WESAD = stress / upper-cusp validation
```

## Cognition

```text
ACDC = broad attentional-control discovery
DMCC or COG-BCI = theory-rich validation / bridge analysis
```

## First-pass analysis

```text
1. Extract features.
2. Run PCA.
3. Run GMM k = 1–10.
4. Run Bayesian GMM.
5. Run HDBSCAN.
6. Compare against simple baselines.
7. Interpret clusters only after inspecting feature profiles and condition enrichment.
8. Decide whether zones are categorical classes, probabilistic regions or continuous-state coordinates.
```

This first cycle should be treated as a **research validation study**, not a production classifier build.

**Footnote — methodological safeguards for cognitive and RR dynamics.** Cognitive DFA estimates will be treated as trial-indexed dynamics unless otherwise stated: the primary `cog_alpha1` analysis uses trial number as the discrete time axis, after residualising task-design effects. Where datasets have highly variable pacing, sensitivity analyses should compare trial-index DFA with time-binned or interpolated series, while noting that interpolation can itself smooth state dynamics. Entropy metrics should not rely solely on a `u_t` series dominated by raw binary accuracy; the primary lag/DFA series may use raw `u_t`, but permutation/difference entropy should also be estimated from continuous or smoothed series, such as residual RT, efficiency, rolling accuracy, or Gaussian-smoothed correctness. For RR data, DFA α1 scale bounds should be pre-specified and tested for sensitivity, because estimates are affected by scale range and series length; the study should report the primary short-scale choice and at least one robustness check. `BODY_RIGID_PERSISTENT` should not be assigned from high `lag1` or α1 alone, and should not require low α1 as a hard threshold. At rest, high lag/persistence may reflect organised vagal/RSA structure rather than rigidity. A rigid-persistent interpretation should require a multivariate profile of persistence with reduced local freedom, reduced entropy/transition diversity, low sign-change or drift-like behaviour, appropriate condition context, and weak evidence for flexible state transition. Conversely, `BODY_ACTIVATED_NEAR_ZONE` may be rare in resting-only datasets and should be expected mainly under low-load task engagement, amusement, recovery-from-activation, or other mildly mobilised conditions. HDBSCAN settings should be pre-specified and subjected to sensitivity checks; an initial rule such as `min_cluster_size = max(10, 0.05 × N_windows)` is acceptable, but should be repeated across nearby values and interpreted alongside GMM/Bayesian GMM stability, not used as a sole decision criterion.

# 9A. Individual-Differences and Personalised Classifier Layer

The public-data validation study primarily tests whether body and cognitive zone structure can be recovered at the population level. However, the eventual Flow Zone classifier should not rely only on fixed population thresholds. Trident-G predicts that individuals differ in their tonic challenge stance, Ψ-band accessibility, recovery dynamics and dominant failure modes. Therefore, the validation protocol should include an individual-differences layer that tests whether state classification improves when population priors are combined with personal baselines and outcome-calibrated routing.

## 9A.1 Core principle

The individual classifier should not ask only:

```text
Is this person high or low relative to the population?
```

It should ask:

```text
Is this person above, below, or displaced from their own regulated-ready range?
Does that displacement predict their cognitive performance, subjective state, recovery and route response?
```

The intended architecture is:

```text
population prior
→ personal baseline learning
→ within-person deviation scoring
→ personalised latent-state classifier
→ outcome-calibrated routing
```

## 9A.2 Population prior phase

Before sufficient personal data exist, the classifier should use the validated population model as a cautious prior.

Early-session outputs should be low-confidence and claim-safe:

```text
This pattern is broadly consistent with a ready / flat / locked / spun-out tendency,
but more personal baseline data are needed.
```

Suggested phase rule:

| Sessions | Classifier behaviour                                                             |
| -------: | -------------------------------------------------------------------------------- |
|      1–3 | Population prior only; very cautious feedback.                                   |
|      4–7 | Provisional personal baselines.                                                  |
|     8–15 | Initial personalised state probabilities.                                        |
|    15–30 | Stronger user-specific state signatures.                                         |
|      30+ | Stable trends, baseline shifts, recovery patterns and personal Ψ-band estimates. |

## 9A.3 Personal baseline features

The classifier should estimate each user’s typical regulated range across body, cognitive, subjective and outcome streams.

### Body stream

```text
personal resting HR range
personal lnRMSSD or RMSSD range where available
personal HRV complexity range
personal DFA α1 range where reliable
personal lag / roughness / entropy profile
personal recovery slope
personal activation tolerance
```

### Cognitive stream

```text
personal response-speed range
personal accuracy range
personal RT variability profile
personal lapse-rate profile
personal control-cost profile
personal post-error slowing profile
personal MFT-M / CCC stability profile
```

### Subjective stream

```text
personal energy anchors
personal focus anchors
personal stress / tension anchors
personal control / readiness anchors
```

### Outcome stream

```text
self-rated session quality
training or work completion
post-route improvement
next-session readiness
next-day recovery
route adherence
route success or failure
```

## 9A.4 Within-person deviation scoring

After provisional baselines exist, each feature should be expressed as both a population-standardised value and a personal deviation value.

Recommended personal score:

```text
personal_z(feature) =
  (current_feature − rolling_personal_median) / rolling_personal_MAD
```

Use exponentially weighted moving averages or rolling windows to track slow baseline change.

The model should distinguish:

```text
state deviation = how unusual today is relative to the person’s normal pattern
trait/profile tendency = the person’s repeated drift pattern across weeks
plasticity trend = whether the person’s regulated-ready baseline is improving
```

## 9A.5 Personal state scores

Before enough data exist for a full personal latent-state model, compute continuous state scores rather than hard labels.

Suggested personal scores:

```text
Ready score
Locked score
Spun-out / scattered score
Flat score
```

The score names may be mapped onto product-safe language later.

Example feature logic:

```text
Ready score =
moderate mobilisation
+ stable accuracy
+ manageable RT variability
+ adequate local flexibility
+ good recovery
+ good subjective focus/control
```

```text
Locked score =
high mobilisation relative to baseline
+ reduced local flexibility
+ elevated control cost
+ perseveration or post-error rigidity
+ high subjective pressure
+ poor recovery
```

```text
Spun-out score =
high volatility
+ high RT variability
+ high lapse rate or unstable accuracy
+ high entropy-like fluctuation
+ subjective agitation or overload
+ poor recovery
```

```text
Flat score =
low mobilisation
+ slow responses
+ low task engagement
+ low throughput
+ high lapse tendency
+ low subjective energy
+ weak adaptation across the task
```

Convert scores into probabilities using a softmax or calibrated probabilistic model:

```text
p_ready
p_locked
p_spun_out
p_flat
confidence
```

## 9A.6 Personal latent-state model

Once sufficient repeated sessions exist, fit a personalised latent-state model using the population classifier as a prior.

Candidate models:

```text
personal Gaussian mixture model
hidden Markov model
Bayesian latent profile model
online logistic regression
Bayesian hierarchical model
```

Preferred research direction:

```text
population-level priors
+ user-specific parameters
+ session-level deviations
+ route-response outcomes
```

The goal is to learn each user’s personal F★ corridor and Ψ-band accessibility:

```text
What does this person’s regulated-ready range look like?
How wide is their Ψ-band?
How quickly do they leave it?
Do they tend to go locked, spun-out or flat?
Which route reliably brings them back?
Is their baseline improving over weeks?
```

## 9A.7 Baseline learning versus plasticity learning

The individual layer should separate two forms of learning.

### Baseline learning

Baseline learning asks:

```text
What is normal for this person?
```

Examples:

```text
usual resting HR
usual lnRMSSD / RMSSD
usual recovery slope
usual MFT-M accuracy
usual RT variability
usual control cost
usual morning/evening difference
```

### Plasticity learning

Plasticity learning asks:

```text
What is changing in this person over weeks?
```

Examples:

```text
baseline in-zone probability increasing
recovery slope improving
less over-bracing after cognitive load
better task stability under challenge
lower route failure rate
faster return from off-zone states
```

This distinction is essential. A one-off readiness score estimates today’s state; a personalised Flow Zone model should also estimate whether the user’s state-regulation capacity is improving over time.

## 9A.8 Route-response calibration

The final product classifier should learn not only the state, but the best route for that state in that person.

Each session should store:

```text
user_id
session_id
timestamp
context: sleep, caffeine, exercise, illness, stress, time of day
body feature vector
cognitive feature vector
subjective ratings
state probabilities
recommended route
route completed: yes/no
post-route state if re-measured
next-session or next-day outcome
```

The key validation question becomes:

```text
Does the personalised state estimate improve route selection beyond a fixed programme or population-only classifier?
```

## 9A.9 Individual-differences outputs

The personalised classifier should eventually report:

```text
current state probability
confidence
usual regulated-ready pattern
dominant drift pattern
best reset / activation / recovery route
recovery trend
baseline in-zone probability
state-transition tendency
route-response history
```

Example product-safe output:

```text
Your current pattern is more consistent with an over-braced / locked tendency than your usual baseline. Your previous sessions suggest that a short downshift route followed by reduced-load work is usually more effective than pushing straight into high demand.
```

## 9A.10 Validation criteria for the individual layer

The individual-differences classifier is supported if:

```text
1. Within-person deviation scores predict outcomes better than population scores alone.
2. Personal baselines improve state-label stability after 8–15 sessions.
3. Personalised state probabilities predict route response better than fixed route assignment.
4. Individual drift profiles are stable enough to classify dominant tendencies.
5. Baseline trends track meaningful improvement or deterioration over weeks.
6. Leave-one-user-out and within-user cross-validation show generalisable benefit.
```

The individual layer is weakened if:

```text
1. Personal baselines add no predictive value beyond population priors.
2. State probabilities remain unstable after sufficient repeated sessions.
3. Route-response predictions fail to improve over time.
4. Apparent individual profiles are explained mainly by sensor artefact, context or time of day.
```

This layer should be treated as the bridge between the public-data validation study and the commercial Flow Zone product.

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
