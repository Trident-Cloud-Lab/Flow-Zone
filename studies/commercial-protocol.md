## Recommended commercial protocol: Flow Check, 6–7 minutes

### Primary version: Polar H10 / chest strap

Polar H10 should be the reference route because your own body spec prefers chest-strap RR intervals/ECG-derived R-peaks, and Polar’s SDK documentation supports heart-rate plus RR-interval streaming in milliseconds.  ([GitHub][1])

| Phase                               |  Duration | User instruction                                                                                                          | Main purpose                                                             |
| ----------------------------------- | --------: | ------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| **1. Signal + context check**       | 20–30 sec | “Sit upright. Keep still. Select today’s context: work / training / recovery / coaching.”                                 | Sensor quality, posture, context, subjective anchors.                    |
| **2. Quiet baseline**               |    90 sec | “Sit still, breathe naturally, eyes open or softly focused.”                                                              | Resting body baseline; avoid paced breathing because it alters HRV.      |
| **3. MFT-M active cognitive probe** | 3:00–3:30 | Adapted MFT-M: Gabor majority decisions, orientation/spatial-frequency switches, easy/hard ratios, no main-task feedback. | MindState classification plus body response under mild cognitive demand. |
| **4. Quiet recovery**               | 75–90 sec | “Stop responding. Sit still and let the system settle.”                                                                   | Recovery slope, persistence of activation, rigidity/chaotic carryover.   |
| **5. Brief subjective check**       | 15–20 sec | Focus, energy, tension/readiness sliders.                                                                                 | User-facing explanation and calibration.                                 |

**Total:** about **6:00–6:40**.

This gives you enough time to approach the body classifier’s minimum of roughly **300 valid NN/RR intervals**, which your spec treats as the minimum for a single body check. At lower resting heart rates, the whole 6-minute recording is important; at higher activation, 300 beats may arrive sooner.  It also gives the MFT-M enough trials to move beyond a very thin express check: your cognitive spec treats 40–50 trials as Express and 80–120 trials as the preferred Core range for dynamics-core features. 

## Why this is better than “everything at rest”

A pure rest protocol is cleaner for traditional HRV, but less useful for **commercial readiness routing**. Flow Zone is not only asking “what is your resting autonomic state?” It is asking:

> “Are you ready to perform, reset, recover or activate?”

That requires seeing how the system behaves under a small standardised challenge. The MFT-M gives a mild cognitive perturbation without the movement artefacts of physical activation. The short recovery period then tells you whether activation is organised and recoverable or whether it remains rigid/noisy.

So the strongest commercial logic is:

```text
resting baseline
+ standardised cognitive challenge
+ immediate recovery
= readiness, reactivity and recovery profile
```

## Active-rest: yes, but make it cognitive, not physical

I would **not** include walking, squats, standing transitions or breathwork inside the measurement phase. Those are too sensor- and context-sensitive, especially for phone PPG. They also make it harder to tell whether the classifier is seeing genuine readiness or just posture/movement artefact.

The “active” component should be the **MFT-M challenge**. The two rest periods should be genuine quiet seated rest.

So the procedure is:

```text
quiet rest → MFT-M cognitive load → quiet recovery
```

not:

```text
physical activation → measurement
```

and not:

```text
five minutes of passive rest only
```

## Phone light/camera fallback

I would treat the phone fallback as **Body-Lite**, not equivalent to Polar H10.

Smartphone camera/flash PPG can be useful under controlled resting conditions, but the evidence is strongest for controlled acquisition and less secure in real-world use; a 2024 scoping review concluded that contact-based smartphone PPG resting heart-rate estimates can agree with ECG in healthy subjects, but mainly under highly controlled conditions. ([Frontiers][2]) Your own body spec also says clean PPG-derived inter-beat intervals are acceptable only when artefact rates are low and device timing is known, and warns against treating heart-rate-only streams as RR data. 

### Fallback protocol: 6–7 minutes

| Phase                         |  Duration | Method                                                      | Output                                                                           |
| ----------------------------- | --------: | ----------------------------------------------------------- | -------------------------------------------------------------------------------- |
| **1. Phone PPG rest**         | 3:00–3:30 | Finger over camera/flash, seated, still, natural breathing. | Body-Lite: HR, RMSSD-like features, signal quality, personal baseline deviation. |
| **2. MFT-M probe**            | 2:45–3:00 | User removes finger and completes the MFT-M.                | MindState: Express/Core-lite depending on trial count.                           |
| **3. Optional post-task PPG** | 45–60 sec | Only if signal quality and time permit.                     | Crude recovery marker, not full recovery modelling.                              |

With phone PPG, the app should show something like:

```text
MindState: available
BodyState: lower-confidence / Body-Lite
Fusion confidence: reduced
```

It should not make the same high-confidence mind–body coupling claims as the Polar version, because the body signal is not collected continuously during the MFT-M.

## Product outputs

The result should expose a simple commercial layer:

| Fusion pattern                                          | User-facing route                  |
| ------------------------------------------------------- | ---------------------------------- |
| Mind in-zone + body ready/near-zone                     | **Perform**                        |
| Mind flat + body ready/rigid                            | **Activate / warm up**             |
| Mind locked-in or body activated-rigid                  | **Reset / widen / downshift**      |
| Mind spun-out or body chaotic-overexcited               | **Recover / stabilise**            |
| Low signal quality or disagreement with weak confidence | **Recheck / interpret cautiously** |

This fits your existing Z-HRV framing: MFT-M cognitive readiness plus RR/HRV-derived readiness plus brief context check gives a small set of actionable readiness/recovery states.  It also preserves the important principle that mind–body disagreement is not noise; it is a useful part of the guidance. 

## Final recommendation

Use this as the standard commercial protocol:

```text
Polar H10 Flow Check
30 sec signal/context
90 sec quiet rest
3–3.5 min MFT-M
75–90 sec quiet recovery
15 sec subjective check
= 6–7 min total
```

Use pure rest-only checks only for **baseline calibration**, not as the main readiness product. A weekly or first-7-sessions “quiet baseline” can help personalise the body model, but the everyday Flow Zone check should include the MFT-M challenge and recovery phase.

[1]: https://github.com/polarofficial/polar-ble-sdk/blob/master/documentation/products/PolarH10.md?utm_source=chatgpt.com "polar-ble-sdk/documentation/products/PolarH10.md ..."
[2]: https://www.frontiersin.org/journals/digital-health/articles/10.3389/fdgth.2024.1326511/full?utm_source=chatgpt.com "Validity of resting heart rate derived from contact-based ..."
