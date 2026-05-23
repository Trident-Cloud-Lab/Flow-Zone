## Phase 1: Construct Validity (Cloud Lab — Between-Subjects)

**Platform**: Prolific or TestMyBrain (web-based, single session, ~25 min, N = 300)

**Goal**: Prove that MFT-GS measures real cognitive constructs and that the 4 behavioral profiles map to meaningful psychological states.

**Protocol**:
1. **MFT-GS** (5 min): Full two-phase protocol. Extract {bits/sec, CV, switch cost, perseveration}.
2. **Independent criterion battery** (15 min):
   - **Processing speed**: Digital Symbol Search or DSST (adapted for touch/mouse).
   - **Cognitive control**: Color-word Stroop or Eriksen flanker (digital).
   - **Task-switching**: Alternating-runs paradigm (switch vs. repeat blocks).
   - **Sustained attention**: Gradual Onset CPT (GradCPT) — a 60-second ultrabrief version validated for remote EMA. 
3. **Self-report state scales** (5 min):
   - Karolinska Sleepiness Scale (KSS)
   - Flow Short Scale
   - Mind-Wandering (MW) probe + intentionality items
   - PANAS (mood)
   - Morningness-Eveningness Questionnaire (chronotype)

**Analyses**:
- **Convergent validity**: MFT-GS bits/sec should correlate with DSST (ρ ≈ 0.4–0.6). Switch cost should correlate with the independent task-switching block (r > 0.3).
- **Divergent validity**: Bits/sec should correlate weakly with trait personality (BFI-10) compared to state sleepiness (KSS).
- **Cluster validation**: Run a Gaussian Mixture Model on the 4 MFT-GS features. Do 4 clusters emerge? Do Cluster 1 ("flat") members report high KSS + low flow? Does Cluster 4 ("locked in") show high perseveration on a post-hoc trail-making task?
- **Discrimination**: Can the 4-profile classifier predict self-reported flow better than chance (AUC > 0.65)?

**Precedent**: Remote digital cognitive tests (e.g., smartphone Stroop + DSST) have achieved ρ = 0.54 with MMSE-2 and AUC = 0.85 for discriminating impaired vs. healthy cognition.  Browser-based psychophysics can achieve <5 ms inter-trial precision using PsychoJS. 

---

## Phase 2: State Sensitivity (App-Based EMA — Within-Subjects)

**Platform**: Your own app distributed to participants' phones. N = 80, 14 days.

**Goal**: Prove that MFT-GS detects *within-person* state fluctuations across the day and responds to experimental manipulations.

**Protocol**:
- **3–4 micro-assessments per day** (fixed times: upon waking, pre-lunch, mid-afternoon, evening).
- Each assessment = MFT-GS (5 min) + 3 EMA items:  
  "Right now I feel focused / scattered / stuck" (1–5).
- **Embedded fatigue manipulation** (randomly assigned days): Before the mid-afternoon assessment, participants complete a 15-minute exhausting GradCPT or SART. This experimentally induces a "flat" or "locked-in" state.
- **Sleep diary**: Time in bed, sleep quality, caffeine intake.

**Analyses**:
- **Multilevel modeling**: Does MFT-GS rate predict within-person variance in self-reported focus (β > 0.3)? Does switch cost predict "scattered" ratings?
- **Manipulation check**: Does the fatigue induction shift the afternoon profile toward "flat" (lower rate, lower CV, higher switch cost) compared to non-manipulation days?
- **Diurnal curves**: Plot bits/sec and state proportions across clock time and biological time (hours since waking). Do you see the expected afternoon dip?
- **Reliability**: Compute ICCs. Between-person reliability of the trait-like rate should be high (>0.80). Within-person reliability of state should be moderate (0.40–0.70), consistent with cognitive EMA literature. 

---

## Phase 3: Recommendation Efficacy (Micro-Randomized Trial in App)

**Platform**: Same app. N = 120, 3–4 weeks.

**Goal**: Prove that the 4-state classification actually *matters* for subsequent performance. This is the ultimate ecological validity test.

**Design**: **Micro-randomized trial (MRT)**. 

**Protocol**:
1. User completes MFT-GS. The classifier outputs a state + a recommendation:
   - In zone → 20-min "deep work" block (hard n-back or insight problems)
   - Flat → 20-min "light work" block (simple sorting or reading)
   - Spun out → 20-min "structured work" block (constrained planning task)
   - Locked in → 20-min "routine work" block (procedural data entry)
2. **Randomization**: On 50 % of sessions (randomly selected), the app **overrides** the recommendation and assigns the *counter-recommended* block type. The user is blind to whether the assignment matches their state.
3. **Outcome**: After the 20-min block, the user completes:
   - A brief **criterion task** (randomly drawn from pool: 2-back d′, creative problem-solving accuracy, or simple RT).
   - NASA-TLX workload scale.

**Analyses**:
- **Primary**: Does following the *true* recommendation (state-matched) yield higher criterion performance than the counter-recommended condition? Test with a weighted and centered least-squares estimator for MRTs. 
- **Moderation**: Is the effect stronger for extreme states (e.g., "flat" → deep work is catastrophic) than borderline cases?
- **Time decay**: Does recommendation efficacy decay over weeks (habituation)? If so, this informs whether the classifier needs to refresh its rules.

**Why this works without lab tech**: The MRT is a purely behavioral, self-contained experiment. Each participant serves as their own control. No imaging needed.

---

## Phase 4: Personalization & Behavioral Criticality Proxies (Longitudinal App)

**Platform**: App-based. N = 50, 6 weeks.

**Goal**: Train the personalized classifier and validate the "criticality" interpretation using behavioral dynamics alone.

**Protocol**:
- **Daily MFT-GS** (as usual).
- **Additional in-app "dynamical" probes** (2 min each, administered 3×/week):
   - **Simple RT task**: 100 trials of tone → tap. Log every RT. Compute:
     - Trial-to-trial RT variability (SD, CV)
     - Spectral slope via Lomb-Scargle periodogram (1/f "pink noise" = criticality proxy)
   - **Tapping task**: Tap index finger at comfortable pace for 2 min. Compute inter-tap interval CV and 1/f slope. (Holden et al. found 1/f scaling in simple motor timing as a criticality marker.) 
   - **Post-tap self-report**: "During that, my mind felt: rigid / flowing / scattered / blank."

**Analyses**:
- **Convergent validity**: Do MFT-GS "in zone" sessions correlate with 1/f pink noise in the tapping task (r > 0.3)? Does "spun out" correlate with white-noise (flat spectral slope) in simple RT? Does "locked in" correlate with brown-noise (steep slope) + low CV?
- **Classifier training**: Feed the first 2 weeks of data into a gradient-boosted model. Weeks 3–6 are the test set. Track classification entropy over time — it should decrease as the model learns the individual's basin structure.
- **Feature importance**: Does switch cost emerge as the strongest predictor of self-reported "stuck" states? Does CV predict "scattered"?

---

## The In-App Criterion Battery (No Lab Tech)

For Phases 1–3, embed these as independent tasks to validate specific components of MFT-GS:

| MFT-GS Claim | In-App Criterion Test | Expected Correlation |
|---|---|---|
| Bits/sec = processing capacity | Digital DSST or Symbol Search | r = 0.4–0.6 |
| Switch cost = cognitive flexibility | Trail Making B (digital) or alternating-runs task | r = 0.3–0.5 |
| CV = arousal/engagement | Psychomotor Vigilance Task (PVT) or KSS | r = −0.4 (high CV ↔ high sleepiness) |
| Perseveration = goal fixation | Wisconsin Card Sorting (simplified) or Stroop interference | r = 0.3 |
| State classification = real state | Flow Scale + MW probe + NASA-TLX after work block | AUC > 0.65 |

---

## Criticality Without EEG: The Behavioral Bridge

You asked how to test the criticality interpretation without neuroimaging. The answer is **convergent dynamics**:

1. **Spectral analysis of response times**: Use the simple RT and tapping tasks to extract 1/f slopes. If "in zone" correlates with pink noise (slope ≈ −1), "flat" with white noise (slope ≈ 0), and "locked in" with brown noise (slope < −1.5), you have behavioral evidence consistent with neural criticality literature. 
2. **Detrended Fluctuation Analysis (DFA)**: Compute long-range temporal correlations (LRTCs) in the MFT-GS trial-to-trial bits/sec series. Critical systems show α ≈ 0.5–0.7.
3. **Multiscale entropy**: "In zone" should show higher complexity (entropy across time scales) than both flat and locked-in states.

These are all computable from timestamps and button presses. No electrodes required.

---

## Summary: Your Validation Stack

| Phase | Method | N | Duration | Cost | Key Output |
|---|---|---|---|---|---|
| **1** | Cloud lab (Prolific) + criterion battery | 300 | 1 session | Low | Construct validity, cluster structure |
| **2** | App EMA + fatigue manipulation | 80 | 2 weeks | Low | State sensitivity, diurnal curves |
| **3** | Micro-randomized trial | 120 | 3–4 weeks | Low | Recommendation efficacy |
| **4** | Longitudinal + dynamical probes | 50 | 6 weeks | Low | Personalized classifier, criticality proxies |

**Total cost**: Essentially zero hardware. Cloud lab fees (~$3–5/participant for 25 min). App hosting. The only development cost is building the tasks, which are all standard HTML5/Canvas/WebGL.

**Timeline**: 6–9 months from launch to publication-ready data. Phase 1 can run immediately; Phase 2–4 can overlap once the app is stable.

The micro-randomized trial (Phase 3) is the most important and underused step in digital cognitive assessment. It moves you from "this classifier outputs labels" to "following these labels improves real-world cognitive performance." That is the standard of evidence needed to justify pre-work guidance. 
