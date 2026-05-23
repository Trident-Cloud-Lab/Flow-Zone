## What You Need

- **Polar H10** (you have this)
- **Raw RR interval data**: Export from Polar Flow or EliteHRV, or capture live via Bluetooth to your phone/app. You need beat-to-beat RR intervals in milliseconds, not just average HR.
- **5–10 minutes of seated, still data** per session
- **Minimum 10–14 sessions** across different times of day and states (tired morning, focused afternoon, post-exercise, pre-work, etc.) to establish your personal baseline

---

## Software to Analyze (Pick One)

| Tool | Best For | Effort |
|---|---|---|
| **Kubios HRV** (free version) | Gold standard for DFA a1, MSE, LF/HF. GUI-based, accepts Polar exports. | Low |
| **Python: `hrv-analysis`** or **`neurokit2`** | Batch processing, custom pipelines, automation. | Medium |
| **Kubios + Excel** | Manual export of metrics, then simple z-score classification. | Low |

I recommend starting with **Kubios free** for the first 2 weeks to validate the concept, then moving to Python if you want to automate it in your app.

---

## The 5-Minute Self-Protocol

**Per session**:
1. Sit upright, feet flat, eyes open.
2. Record 5 minutes. No phone, no talking.
3. Log immediately after:
   - Time of day, hours since waking
   - Subjective state (1–5): sharp, scattered, stuck, sluggish
   - What you did next (work type) and how it felt (optional)

**Do this**:
- Morning (pre-coffee), afternoon (post-lunch), evening
- At least one session when you feel "in flow"
- At least one when you feel exhausted or "scattered"
- At least one when you feel "stuck" on a problem

---

## Metrics to Extract Per Session (Kubios Workflow)

1. **Import** RR data → Kubios
2. **Artifact correction**: Use "Very low" threshold (automatic)
3. **Time-domain**: Note **RMSSD**
4. **Frequency-domain**: Note **LF/HF ratio** (and LF, HF power in ms²)
5. **Nonlinear**:
   - **DFA**: Note **Short-term scaling exponent (a1)** (window 4–16 beats)
   - **Sample Entropy**: Note **SampEn** (m=2, r=0.2)
   - **Multiscale Entropy**: If available, note the **Complexity Index** (area under MSE curve, scales 1–5)

---

## Quick Classification on Your Own Data

After 10–14 sessions, compute your **personal means and SDs** for each metric:

```python
# Pseudocode for your spreadsheet
if (DFA_a1 between 0.85 and 1.15) and (SampEn > mean + 0.3*SD):
    state = "IN_ZONE"
elif (RMSSD < mean - 0.5*SD) and (LF_HF < mean - 0.5*SD) and (SampEn < mean - 0.5*SD):
    state = "FLAT"
elif (DFA_a1 < 0.75) and (LF_HF > mean + 0.5*SD):
    state = "SPUN_OUT"
elif (DFA_a1 > 1.2) and (SampEn < mean - 0.5*SD):
    state = "LOCKED_IN"
else:
    state = "UNCERTAIN"
```

**Then validate against your subjective logs**:
- Did "in zone" sessions correlate with high focus ratings?
- Did "flat" sessions correlate with sluggishness?
- Did "spun out" correlate with scattered mind ratings?
- Did "locked in" correlate with feeling stuck?

---

## What You Can Learn in 2 Weeks

| Question | Evidence |
|---|---|
| Does my HRV complexity predict how I feel? | Correlate SampEn/CI with subjective ratings (target: r > 0.4) |
| Does DFA a1 ≈ 1.0 map to good days? | Count how many "good work" sessions fall in the 0.85–1.15 band |
| Can I detect post-lunch dip? | Plot DFA a1 and RMSSD by time of day |
| Does caffeine/food/sleep change my state? | Compare sessions with different prior conditions |

---

## Critical Realistic Notes

1. **N = 1, no blinding**: You know your own ratings, so expectancy effects are real. Treat this as **feasibility testing**, not validation. It tells you if the metrics *move* with your experience, not if they are objectively true.
2. **10 sessions is the floor**: You need enough spread to see your personal range. If all 10 sessions are at the same time of day in the same mood, you will see nothing.
3. **Kubios free limitations**: The free version may limit batch export. You may need to manually transcribe 4 numbers per session into a spreadsheet. That's fine for a pilot.
4. **DFA a1 can be noisy session-to-session**: Don't over-interpret a single outlier. Look for patterns over 3–4 day blocks.

---

## The 30-Day Path to an App Feature

If your self-data looks promising (i.e., DFA a1 and SampEn track your subjective states with better-than-chance accuracy), the next steps are:

- **Weeks 1–2**: Self-experiment as above. Build spreadsheet. Validate heuristic.
- **Weeks 3–4**: Add the **MFT-GS cognitive task** immediately after HRV recording. See if cognitive and physiological states converge.
- **Month 2**: If convergence is >70 %, design the app integration: 5-min HRV → auto-classification → work recommendation.

You can start the HRV-only pilot **today** with data you already have, or with one 5-minute recording right now. The barrier is essentially zero.
