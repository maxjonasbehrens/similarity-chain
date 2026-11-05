# Worked Example: Framingham Risk Score Transport

> **Scope note.** This is analytical task 2.1 (transport a prediction model), an
> EXTENSION task outside the paper's combined-data scope. The protocol mechanics
> shown here are identical for in-scope tasks, but do not cite this as an example
> of what the paper claims.

This example walks through the full similarity chain protocol for the canonical
case of transporting the Framingham cardiovascular risk score to a Chinese population.

**Sources:** D'Agostino et al. (2001), Liu et al. (2004)

---

## Step 1/6: ELICIT

**Q1: What is your analytical task?**

> We want to apply the Framingham Risk Score — a Cox regression model for 10-year
> coronary heart disease (CHD) risk developed from the Framingham Heart Study cohort
> in the US — to predict CHD risk in a Chinese population cohort.

**Q2: Why integrate rather than analyze locally?**

> The Chinese cohort has follow-up data and CHD events, but developing a new Cox model
> from scratch would require substantially more events for stable estimation of 7
> predictor coefficients. Transporting the established Framingham model — if defensible
> — leverages decades of development and validation work.

**Q3: What do you know about both contexts?**

> - **Source (Framingham):** Prospective cohort study, predominantly white participants
>   from Framingham, Massachusetts. ~5,000 participants, decades of follow-up. Outcome:
>   10-year CHD risk. Predictors: age, sex, total cholesterol, HDL cholesterol, systolic
>   BP, smoking, diabetes.
> - **Target (Chinese cohort):** Population-based cohort from China. Different ethnic
>   composition, different baseline CHD incidence rates, different dietary and
>   environmental exposures. Similar clinical variables measured.

**Q4: What data/metadata do you have access to?**

> Published model coefficients (beta) and baseline survival function S_0(t) from
> Framingham. Raw data from the Chinese cohort including all predictor variables and
> CHD outcomes. Published validation results from Liu et al. (2004).

**Reflect-back:**

> **What I understood:**
> - **Analytical task:** Transport the Framingham 10-year CHD risk model to a Chinese cohort
> - **Why integration is needed:** Developing a new model from scratch requires more
>   events than available; Framingham provides established coefficients
> - **Source:** Framingham Heart Study, US, n ≈ 5,000, predominantly white
> - **Target:** Chinese population-based cohort, different ethnic/dietary/environmental context
> - **Available data:** Published Framingham model (betas + S_0(t)), raw Chinese cohort
>   data, published validation results
> - **Feasibility note:** Both cohorts are large, so dissimilarity functions computed
>   from data are feasible for the data and model subspaces. The design subspace rests
>   on published protocols, so its condition can be assumed but not met unless it is
>   measured. This is an unusually data-rich setting for model transport — most
>   transport decisions have less evidence available.

---

## Step 2/6: CLASSIFY

**Task catalog match:** 2.1 Transport Prediction Model

**Why this match:** The researcher has a fully specified model (coefficients and baseline
hazard) from one population and wants to apply it as-is to make predictions in another.
This is model transport, not external validation (2.2) — the goal is deployment, not just
evaluation. Not transfer learning (2.4) — no retraining is planned.

**Required subspaces (in assessment order):**
1. Design: operationalization (gate) — *are CHD and the risk factors the same thing in both studies?*
2. Data: distributional (graded check) — *do the patient populations overlap enough?*
3. Model: predictive (graded check) — *does the model's conditional relationship P(CHD|predictors) hold?*

---

## Step 3/6: ROADMAP

**Gated pipeline:**

```
Operationalization ──gate──→ Distributional ──→ Predictive
        │                          │                  │
    If fails:                  If fails:          If fails:
    STOP.                      Flag: model may    Cannot transport
    Model predicts             extrapolate;       without modification
    wrong endpoint             calibration at
                               risk
```

**Decision rule** (applied in Step 5):
- If the operationalization gate is not met → cannot transport without a structural remedy
- If any condition is assumed → Assumption-based Integration
- If distributional or predictive similarity conditions fail → Integration with Modifications
- The first matching rule decides, and there is exactly one recommendation

**Small-data note:** Not applicable in this example — both cohorts are large. In a
typical small-data transport scenario, distributional dissimilarity functions would have
wide CIs and the predictive similarity condition might degrade to assumed if the target
has too few events for reliable AUC estimation.

**Pattern alert:** This case itself *became* a canonical failure case — the Framingham
score was widely transported without assessing calibration, leading to systematic
overestimation in non-Western populations. The protocol would have predicted this.

---

## Step 4/6: ASSESS

### Assessment 1/3: Design — Operationalization [gate]

> **This is a gate** — if outcome or predictor definitions differ, the model
> predicts a different endpoint in China than it was built for. Downstream
> assessments would be meaningless.

**Similarity condition:** Are "10-year CHD risk" and the 7 predictor variables
measuring the same constructs, on the same scales, with the same measurement-error
type in both the Framingham cohort and the Chinese cohort?

**Evidence:** The published protocols of both studies use clinical CHD events (MI,
CHD death) as outcomes. Risk factors (age, cholesterol, BP, smoking, diabetes) are
measured using standard clinical assessments. Both use continuous BP and cholesterol,
both use binary smoking and diabetes.

**Status: Assumed**

The protocols agree: both describe clinically ascertained CHD events and standard
clinical measurements. Minor protocol differences in ascertainment methods exist.
Agreeing documents cannot show that the two studies measured what the documents say,
so the condition is assumed, not met.

**Dissimilarity function:** Scale-alignment proportion = 1.0 (all predictors use
matching scales, according to the protocols).

**What would establish it:** Re-adjudicating a sample of CHD events from both cohorts
against one common definition, and comparing BP and cholesterol measurements against
shared calibration data.

**Gate: ASSUMED** — proceed to distributional assessment, conditional on this assumption.

> **Progress:**
>
> | # | Subspace | Status | Role | Implication |
> |---|----------|--------|------|-------------|
> | 1 | Operationalization | Assumed ○ | Gate — assumed | Proceed, conditional on assumption |
> | 2 | Distributional | *up next* | — | — |
> | 3 | Predictive | *pending* | — | — |

---

### Assessment 2/3: Data — Distributional [graded check]

> Taking the outcome and predictors to mean the same thing (assumed above), we can
> compare whether the patient populations overlap.

**Similarity condition:** Do the predictor values and CHD event rates in the Chinese
cohort fall within the range the Framingham model was trained on?

**Evidence:** Published baseline characteristics show substantial differences:
- CHD incidence in Chinese cohort is much lower than in Framingham.
- Cholesterol distributions differ (lower in Chinese population).
- Smoking prevalence differs by sex across populations.
- Baseline hazard rate S_0(t) differs dramatically.

**Status: Not met**

Variables are in correspondence (signature equivalent) and on the same scales, but
the empirical distributions differ substantially. Most critically, the baseline
hazard rate — which determines absolute risk predictions — is much lower in the
Chinese cohort.

**Dissimilarity functions:**
- Covariate SMDs: Vary by predictor; several exceed 0.1 threshold.
- Baseline CHD incidence: ~276% overestimation when using Framingham S_0(t).

**Concern flagged.** Distributional dissimilarity does not block model transport
(graded check), but it signals that calibration will likely fail even if the model
relationship transfers. We'll carry this into the recommendation — modification
(recalibration of the baseline hazard) will likely be needed.

> **Progress:**
>
> | # | Subspace | Status | Role | Implication |
> |---|----------|--------|------|-------------|
> | 1 | Operationalization | Assumed ○ | Gate — assumed | Proceed, conditional on assumption |
> | 2 | Distributional | Not met ✗ | Graded check | Modification needed |
> | 3 | Predictive | *up next* | — | — |

---

### Assessment 3/3: Model — Predictive [graded check]

> We know the populations differ substantially. The question now is whether the
> model's *conditional* relationship — how it ranks and predicts patients — still
> holds despite those differences.

**Similarity condition:** Is P(CHD | predictors) the same in both contexts?
Specifically: does the model rank patients correctly (discrimination), and are its
absolute predictions accurate (calibration)?

**Evidence:** Published cross-application results from Liu et al. (2004):
- **Discrimination (model-subspace property):** AUC in Chinese men = 0.705 vs. 0.736
  in Framingham. Preserved — the model ranks patients by risk similarly in both
  contexts.
- **Calibration (data-subspace property):** Catastrophic failure. The Framingham
  baseline hazard overestimates 10-year CHD risk in the Chinese cohort by
  approximately 276%. Predicted risks are far higher than observed.

**Status: Not met**

The condition requires P(CHD | predictors) to be the same in both contexts. The
model's ranking transfers, but its absolute predictions do not, so the condition fails.

- Discrimination: preserved — the model's ranking ability transfers.
- Calibration: fails — absolute risk predictions are wrong due to different
  baseline hazard.

**Dissimilarity functions:**
- Discrimination difference: |AUC_source - AUC_target| ≈ 0.03 (acceptable).
- Calibration-in-the-large: Observed/Expected ratio far from 1.0.
- Calibration slope: Preserved (beta coefficients transfer).

**Key insight:** Discrimination and calibration belong to **different subspaces**.
Discrimination is a model-subspace property (P(Y|X) ranking transfers). Calibration
depends on the baseline hazard S_0(t) — a data-subspace property. The modification
(D'Agostino recalibration) operates on the data subspace while leaving the model
subspace untouched.

**Concern flagged.** Calibration failure requires modification: recalibrate the
baseline hazard using target-context data.

> **Progress (final):**
>
> | # | Subspace | Status | Role | Implication |
> |---|----------|--------|------|-------------|
> | 1 | Operationalization | Assumed ○ | Gate — assumed | Proceed, conditional on assumption |
> | 2 | Distributional | Not met ✗ | Graded check | Modification needed |
> | 3 | Predictive | Not met ✗ | Graded check | Modification needed |

---

## Step 5/6: RECOMMENDATION

**ASSUMPTION-BASED INTEGRATION**

The operationalization gate rests on agreeing protocols, so it is assumed. Two graded
checks are not met and require modification. The Framingham model can be transported
to the Chinese cohort under the stated assumption, and calibration must be modified.

**Basis:**
- Gates: 1 assessed, 0 met, 1 assumed, 0 not met
- Graded checks: 2 assessed, 0 met, 0 assumed, 2 not met (distributional + predictive)
- Deciding rule: rule 3 (a gate is assumed). Rule 4 would also match, so the
  modifications are still required, but rule 3 matches first.

**Assumption inventory:**
1. Operationalization — CHD events and risk factors are defined and measured the same
   way in both cohorts. Assumed because only the published protocols support it.

**Required modifications:**
1. **Recalibrate baseline hazard** — re-estimate S_0(t) from target-context data
   while holding beta coefficients fixed (D'Agostino recalibration). This operates
   on the data subspace without modifying the model subspace.
2. **Post-recalibration validation** — re-evaluate calibration curves to confirm
   the modification was effective.

**What this means concretely:** Transport the Framingham model coefficients as-is,
but do *not* use the Framingham baseline hazard for absolute risk predictions.
Estimate S_0(t) from the Chinese cohort. Report discrimination (which transfers)
and recalibrated calibration separately. State the operationalization assumption in
the methods section.

---

## Step 6/6: REPORT

# Similarity Specification Report

## Task and context

- **Analytical task:** Transport the Framingham Risk Score (10-year CHD risk, Cox
  model) from the Framingham Heart Study (US) to a Chinese population cohort.
- **Task catalog match:** 2.1 Transport Prediction Model
- **Source context:** Framingham Heart Study, predominantly white US population,
  n ≈ 5,000, prospective cohort with decades of follow-up.
- **Target context:** Population-based Chinese cohort, n = [reported in Liu 2004],
  prospective design with CHD outcomes.
- **Reason for integration:** Leveraging the established, extensively validated
  Framingham model rather than developing a model from scratch. Sample size is not the
  primary driver: the Chinese cohort has sufficient events for validation. In a true
  small-data scenario (e.g., n < 200, < 20 events), more similarity conditions would
  degrade to "assumed."
- **Feasibility boundary:** Both cohorts are large, so the data and model conditions
  are empirically assessable. The design condition rests on published protocols and
  can only be assumed.

---

## Subspace assessments

### 1. Operationalization: operational equivalence

- **Role:** gate
- **Condition:** CHD outcomes and risk factors defined and measured the same way in
  both studies.
- **Status:** assumed
- **Evidence:** Published protocols. Both describe clinically ascertained CHD events
  (MI, CHD death) and standard clinical assessments of all predictors on matching scales.
- **Dissimilarity function (with uncertainty):** Scale-alignment proportion = 1.0,
  from the protocols (no sampling uncertainty).
- **If assumed, evidence that would establish it:** Re-adjudication of a sample of CHD
  events from both cohorts against one common definition, and BP and cholesterol
  measurements compared against shared calibration data.

### 2. Distributional: distributional similarity

- **Role:** graded check
- **Condition:** Predictor and outcome distributions overlap sufficiently for model
  application.
- **Status:** not met
- **Evidence:** Baseline CHD incidence substantially lower in Chinese cohort.
  Several predictor distributions differ (cholesterol, smoking patterns).
  Baseline hazard S_0(t) differs dramatically.
- **Dissimilarity function (with uncertainty):** ~276% overestimation of CHD risk using
  Framingham S_0(t). Multiple covariate SMDs > 0.1.

### 3. Predictive: predictive exchangeability

- **Role:** graded check
- **Condition:** P(CHD | predictors) is invariant across contexts.
- **Status:** not met (discrimination preserved, calibration fails)
- **Evidence:**
  - Discrimination preserved (AUC: 0.705 vs. 0.736).
  - Calibration fails (O/E ratio far from 1.0).
- **Dissimilarity function (with uncertainty):** |AUC difference| ≈ 0.03;
  calibration-in-the-large fails.

---

## Recommendation

- **Recommendation:** Assumption-based Integration
- **Basis:** Rule 3 is the first matching rule, because the operationalization gate is
  assumed. Both graded checks are not met (rule 4 would also match).
- **Required modifications:**
  - **Recalibrate baseline hazard:** Re-estimate S_0(t) from target-context data
    while holding beta fixed (D'Agostino recalibration). This operates on the data
    subspace without modifying the model subspace.
  - **Post-recalibration validation:** Re-evaluate calibration curves to confirm
    the modification was effective.
- **Assumption inventory:** Operationalization. CHD events and risk factors are defined
  and measured the same way in both cohorts. Assumed because only the published
  protocols support it.
- **Gate remediations:** None.

---

## Suggested methods-section paragraph

> We transported the Framingham Risk Score for 10-year CHD risk to a Chinese
> population cohort. To assess whether this transport was defensible, we applied
> the similarity chain protocol. The operationalization similarity condition was
> assumed: the published protocols of both studies describe clinically ascertained
> CHD events and standard clinical measurements (scale-alignment proportion = 1.0),
> but documents cannot show that both studies measured what they describe. The
> distributional similarity condition was not met: baseline CHD incidence was
> substantially lower in the target population, with approximately 276%
> overestimation using the source baseline hazard. The predictive similarity
> condition was not met: discrimination was preserved (AUC = 0.705 in the Chinese
> cohort vs. 0.736 in Framingham) but calibration failed. The recommendation was
> Assumption-based Integration. Following D'Agostino et al. (2001), we recalibrated
> the baseline survival function S_0(t) using target-context data while retaining the
> original regression coefficients, restoring calibration while preserving the
> transferred model structure.

---

## Metadata

- **Date:** 2026-02-17
- **Assessed by:** AI-assisted via similarity chain protocol
- **Data access:** mixed (published model coefficients and validation results,
  target-context raw data)
- **Local sample size:** Large (both cohorts); not a small-data constraint

---

## Teaching Notes

This example illustrates several key principles:

1. **Discrimination ≠ calibration.** The model's ranking works across contexts,
   but absolute predictions fail. These are different subspace properties.

2. **Data dissimilarity + model similarity.** The subspaces are independent.
   Different distributions (data) can coexist with the same conditional
   relationship (model).

3. **The modification confirms independence.** Recalibration fixes a data-subspace
   quantity (S_0(t)) while leaving a model-subspace quantity (beta) untouched.
   If the subspaces were coupled, this modification would not work.

4. **The protocol would not have prevented this application** — it would have
   *predicted the calibration failure* and recommended recalibration before
   deployment, rather than discovering it after the fact.

5. **This is an unusually data-rich example.** In small-data settings (n < 200,
   few events), distributional and predictive dissimilarity functions would degrade:
   SMDs would have wide CIs, AUC estimation would be unreliable, and more
   similarity conditions would be classified as "assumed" rather than "met." The
   recommendation would stay ASSUMPTION-BASED INTEGRATION, but more of the claim
   would rest on the assumption inventory.

6. **The first matching rule decides.** Agreeing protocols leave the design gate
   assumed, so rule 3 decides before rule 4 is reached. The failed graded checks
   still appear in the report, and their modifications are still required.
