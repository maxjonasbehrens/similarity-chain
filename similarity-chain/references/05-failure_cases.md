# Canonical Failure Cases

Three real-world failures where similarity misalignment caused harm. Each illustrates
the same structural pattern: similarity was verified in one subspace and assumed
sufficient for a decision that depended on another.

Use these cases to calibrate your assessments. If the researcher's situation resembles
any of these patterns, flag it explicitly.

---

## Case 1: Epic Sepsis Prediction Model

**Source:** Wong et al. (2021), Lyons et al. (2023)

### What Happened

A proprietary sepsis prediction model was deployed across hundreds of US hospitals.
The justification: all sites used the same EHR system, the same input features, and
served patient populations with comparable demographics.

External validation revealed poor performance at multiple sites.

### What Was Assessed

- **Data: signature** — Same EHR system, same input features. ✓
- **Data: distributional** — Comparable demographics across sites. ✓

### What Was Needed

- **Design: operationalization** — Outcome definitions, documentation practices,
  alert thresholds, and coding conventions varied between sites. The "same features"
  represented semantically different things.

### Protocol Diagnosis

The integration decision was **transport prediction model** (catalog entry 2.1).
The protocol requires: **operationalization (gate) → distributional → predictive**.

The gate (operationalization) was skipped. Data similarity was verified, but
the features overlapped syntactically, not semantically. Because the gate's
similarity condition was not met, all downstream assessments (distributional
overlap, predictive performance) were built on an invalid foundation.

### What the Protocol Would Have Flagged

Step 3 (ROADMAP) would present operationalization as the first gate:

> "Before assessing distributional overlap or predictive performance, we need to
> verify that outcome definitions and feature meanings are the same across sites.
> This is a gate — if the similarity condition fails, distributional and predictive
> assessments are not meaningful."

The researcher would be asked: "Are sepsis outcomes defined identically across sites?
Are documentation practices standardized?" The answer would reveal the dissimilarity
before any model was deployed.

---

## Case 2: Mouse Inflammation Models

**Source:** Seok et al. (2013)

### What Happened

Approximately 150 clinical trials of anti-inflammatory agents for sepsis, burns,
and trauma failed after development based on mouse model efficacy. The mouse and
human genomes share conserved immune pathways, so molecular overlap was taken as
evidence that findings would translate.

### What Was Assessed

- **Data: distributional** — Molecular/genomic overlap between mouse and human
  immune pathways. ✓ (partially — conserved pathways exist)
- **Data: signature** — Homologous gene sets could be placed in correspondence. ✓

### What Was Needed

- **Design: operationalization** — Housing conditions, injury protocols, and
  disease induction methods in mice bear no resemblance to human disease onset.
  The "same construct" (inflammation/sepsis) was operationalized entirely differently.
- **Model: causal** — The causal mechanisms driving inflammation differ between
  controlled mouse injury and spontaneous human sepsis. Structural similarity of
  the causal model was assumed but not supported.

### Protocol Diagnosis

The integration decision was **transport causal effect** (catalog entry 4.3).
The protocol requires: **causal (gate) → sampling → distributional**.

The causal gate comes first, and the conserved pathways that were assessed cannot
establish it. The catalog entry does not list operationalization, but adapting the
entry into a custom chain (Step 2) would have added it. That assessment would have
revealed that "sepsis" in a mouse model and "sepsis" in a human ICU are different
constructs, a design-level failure that undermines everything downstream.

### What the Protocol Would Have Flagged

Step 2 (CLASSIFY) would identify this as causal effect transport. Step 3 would
present the assessment sequence. But a careful ELICIT (Step 1) would surface the
cross-species nature of the integration, triggering an operationalization assessment:

> "The constructs 'mouse inflammation from controlled burn injury' and 'human
> sepsis from spontaneous infection' are not operationally equivalent. This is
> a gate failure at the design level."

The protocol would also flag causal model similarity as unverifiable: causal structure
in mouse models cannot be assumed to match human pathophysiology without explicit
evidence.

---

## Case 3: Pneumonia Detection Imaging

**Source:** Zech et al. (2018)

### What Happened

A deep learning model for pneumonia detection on chest X-rays achieved high accuracy
within the training hospital but degraded substantially when applied to images from
other hospitals. Investigation revealed the model exploited scanner metadata and
hospital-specific imaging artifacts rather than lung pathology.

### What Was Assessed

- **Model: predictive** — Within-site performance (AUC, accuracy). ✓
  But this was native performance, not cross-site evaluation.

### What Was Needed

- **Design: operationalization** — Different scanners, imaging protocols, and
  preprocessing pipelines meant the "same" image type (chest X-ray) was
  operationalized differently across sites. Scanner metadata encoded site identity.
- **Data: distributional** — Cross-site distributional comparison would have
  revealed that image feature distributions were site-specific.
- **Model: predictive** — Cross-application evaluation (train on site A, test on
  site B) was needed but not performed until after deployment.

### Protocol Diagnosis

The integration decision was **transport prediction model** (catalog entry 2.1).
The protocol requires: **operationalization (gate) → distributional → predictive**.

Only native predictive performance was assessed — the model was evaluated where it
was trained. The operationalization gate (are the images measuring the same thing
across sites?) was never assessed. Cross-site distributional comparison was never
performed. Cross-application predictive evaluation was never performed.

### What the Protocol Would Have Flagged

Step 3 (ROADMAP) would present the ordered sequence:

> "1. Operationalization: Are chest X-rays acquired with the same scanner types,
>    protocols, and preprocessing across sites? (GATE)
>  2. Distributional: Do image feature distributions overlap across sites?
>  3. Predictive: Does the model maintain discrimination and calibration when
>     applied cross-site?"

The operationalization assessment would surface scanner and protocol differences. Even
if the gate were considered passable (all are "chest X-rays"), the distributional
assessment would reveal site-specific feature distributions. And the predictive
assessment would require cross-application evaluation, not within-site testing.

---

## Common Pattern

All three failures share the same structure:

1. **The similarity that was easy to assess was assessed** (data overlap, within-site
   performance, molecular homology).
2. **The similarity that the decision required was not assessed** (operationalization,
   causal mechanism, cross-site performance).
3. **Data availability dictated the comparison** rather than the decision dictating it.

The protocol prevents this by starting from the decision (Step 2: CLASSIFY) and
deriving the required subspaces, rather than starting from available data and
assessing whatever is convenient.

### Red Flags to Watch For

When assessing a researcher's case, flag these patterns:

- **"We have the same variables"** — Signature similarity does not imply
  operationalization similarity. Same column names ≠ same constructs.
- **"The model works well here"** — Native performance does not predict
  cross-context performance. Always require cross-application evaluation.
- **"The biology is conserved"** — Molecular overlap does not imply causal
  similarity. Conserved pathways may be regulated differently in different
  organisms or conditions.
- **"The demographics are similar"** — Distributional overlap does not imply
  design similarity. Similar populations may have been measured differently.
- **"We checked covariate balance"** — Balance is a data-subspace similarity
  measure used as a proxy for a design-subspace similarity condition. State the
  cross-subspace reasoning explicitly.
