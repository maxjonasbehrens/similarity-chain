# Canonical Failure Cases

Three real-world failures where similarity misalignment led to a wrong integration
decision. Each illustrates the same structural pattern: similarity was verified in one
subspace and assumed sufficient for a decision that depended on another.

Use these cases to calibrate your assessments. If the researcher's situation resembles
any of these patterns, flag it explicitly.

---

## Case 1: Coding Revision in US Mortality Data

**Source:** Anderson et al. (2001), the comparability study of the National Center for
Health Statistics (NCHS). The paper's supplement works this case through the chain
("Coding revisions in mortality data").

### What Happened

In 1999 the United States moved from the ninth to the tenth revision of the
International Classification of Diseases (ICD). An analyst who trends cause-specific
mortality across that year splices a series across a definitional change. NCHS
publishes a comparability ratio for each cause: the count under ICD-10 divided by the
count under ICD-9, on the same deaths. The field's remedy rescales each cause by its
ratio and trends on.

NCHS coded 2,296,922 deaths from 1996 under both revisions and released the result.
For every death, the file shows what it was called under the old rules and what it is
called under the new ones. Of those deaths, 171,872 (7.48%) change category on the
standard NCHS list of 113 causes.

### What Was Assessed

- **Comparability ratio, per cause.** The ratio reports how each category's total
  changed. Computed on the dual-coded file, it reproduces the published values, for
  example 0.695 for pneumonia and 1.581 for Alzheimer's disease. ✓

### What Was Needed

- **Design: operationalization (gate).** Cause categories must be assigned under the
  same coding rules on both sides of the break. The classification system changed,
  and the codebook shows this before any record is read. The gate is not met, and the
  remediation it calls for is a verified correspondence between categories.
- **Data: signature (gate).** Each cause must map onto one category in both
  directions. Two quantities assess this per cause, from the same published file as
  the ratio. *Retention* is the share of a category's ICD-9 deaths that remain in it
  under ICD-10. *Purity* is the share of its ICD-10 deaths that were in it under ICD-9.

### Protocol Diagnosis

The analytical task was **splice a series across a regime change** (catalog entry
1.5). The protocol requires: **operationalization (gate) → signature (gate) →
distributional**.

The field assessed a ratio of totals where the task needs the signature gate. A ratio does not report *why* a
total changed, and two opposite mechanisms produce a comparable departure from unity:

| Cause | Retention | Purity | Ratio | Mechanism |
|---|---|---|---|---|
| Pneumonia | 67.7% | 97.4% | 0.695 | deaths leave the category |
| Alzheimer's disease | 96.7% | 61.2% | 1.581 | deaths arrive from elsewhere |

Alzheimer's disease keeps almost all of its own deaths, yet 38.8% of its ICD-10 deaths
come from outside, largely from the residual "all other diseases" category. Rescaling
its ICD-9 series by 1.581 assumes that the imported deaths move in step with the
category's own deaths in every year. A single dual-coded year cannot show that. A
ratio near unity does not show an intact correspondence either. Alcoholic liver
disease has a ratio of 1.001, yet its retention and purity are both about 87%.

The dual coding supplies the known answer. The departure of the ratio from unity is
strongly rank-correlated with the failure of correspondence (Spearman ρ = -0.908
against the smaller of retention and purity). The usual check rescales and trends
exactly the causes that should be reported as discontinuous. A distributional check
would not have helped. Comparing the two eras on age, sex, race, state and education
returns a standardized difference of exactly zero on every variable, because the two
codings describe the same people.

### What the Protocol Would Have Flagged

Step 3 (ROADMAP) would present two gates before any distributional comparison:

> "1. Operationalization: Were cause categories assigned under the same coding rules
>    on both sides of the 1999 break? (GATE)
>  2. Signature: Does each cause map onto one category in both directions? (GATE)
>  3. Distributional: Are age, sex, race, state and education distributed comparably
>     across the eras?"

The operationalization gate fails on the codebook alone. The signature gate then turns
the blanket rescaling into a recommendation per cause. Of the 59 causes with at least
5,000 ICD-9 deaths:

- 21 have retention and purity both at or above 95%. They cover 46.0% of the ICD-9
  deaths in the 59 causes, and their ratios lie between 0.979 and 1.036. The
  recommendation is Integration with Remediation, the remediation being the verified
  correspondence.
- 24 have either measure below 90%. Their ratios lie between 0.695 and 1.632. The
  recommendation is Integration not recommended, and the series is reported as
  discontinuous.
- 14 lie between the thresholds. The recommendation is also Integration with
  Remediation, because the design gate fails. Their correspondence is assumed rather
  than verified. The series may be trended, with the correspondence stated as an
  assumption and both measures reported beside it.

The classification is available before any trend is estimated. Two limits apply. The
catalog entry for this task was written while the case was under study, so the case
shows that the conventional remedy is insufficient. It does not show that the protocol
would have identified the task unprompted. The obvious remediation, a coarser
classification, also fails here. Collapsing the 113 causes into 16 groups by ICD
chapter raises the share of all deaths that keep their category only from 92.5% to 95.2%,
and it removes one-to-one correspondence entirely.

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
  controlled mouse injury and spontaneous human sepsis. Structural causal similarity
  was assumed but not supported.

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

1. **The similarity that was easy to assess was assessed** (a ratio of category totals,
   within-site performance, molecular homology).
2. **The similarity that the decision required was not assessed** (category
   correspondence, causal mechanism, cross-site performance).
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
- **"The provider publishes a correction factor"**: A ratio of totals does not show
  that categories correspond. Where records are coded under both schemes, report
  retention and purity per category.
