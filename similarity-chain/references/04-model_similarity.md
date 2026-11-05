# Model Similarity

Model similarity concerns whether the relationships a model encodes — predictions,
effects, causal mechanisms — transfer across contexts. The integration decision
determines which aspect of model behavior matters:

- Transporting a prediction rule → P(Y|X) must be approximately invariant.
- Generalizing a treatment effect → the causal structure must transfer. Whether effect
  magnitudes must also match depends on the task (Subcategory 2).

Any model can be projected onto the decision-relevant aspect: a projection P_a(M)
extracts the relevant representation, and a dissimilarity function d_a(M1, M2)
quantifies disagreement on that aspect. The protocol requires both the aspect and the
dissimilarity function be stated explicitly.

---

## Subcategory 1: Predictive Model Similarity

### Similarity Condition

Two contexts are **predictively exchangeable** if the conditional distribution P(Y|X)
is the same in both, where Y is the outcome and X the predictor set relevant to the
decision.

**Key distinctions:**
- Predictive exchangeability does **not** require that marginal distributions P(X)
  match. Covariate shift is compatible with predictive exchangeability as long as
  the conditional relationship is preserved, but it can change measures that depend
  on the case mix, such as discrimination.
- **Discrimination and calibration must be reported separately.** Discrimination
  depends on the model and on the case mix. A calibration failure often reflects a
  different baseline risk at the same predictor values. That difference lies in
  P(Y|X) and counts against predictive exchangeability.

**Discrimination can transfer while calibration fails:**
A model can keep its discrimination (c-statistic) in a new context while its
calibration fails, because the baseline risk differs at the same predictor values.
Recalibration re-estimates the baseline hazard S_0(t) and holds the coefficients
beta fixed. It updates the model but keeps the relative effects of the predictors.

### Dissimilarity Functions

1. **Cross-application error (non-native penalty).**
   Fit model in context A, evaluate in B (and vice versa). The difference between
   native and non-native performance quantifies the transferability penalty.
   - *Central dissimilarity function for transport decisions.*
   - Report both directions (A→B and B→A) — asymmetry is informative.

2. **Discrimination differences.**
   Compare c-statistic, AUC, or concordance index of the same model in both contexts.
   - Discrimination depends on the case mix. Compare the observed c-statistic with
     the case-mix-corrected c-statistic, the value expected in the new case mix if
     the coefficients hold.
   - c-statistic near 0.5 = no discrimination.
   - Report difference with CI: |AUC_A - AUC_B| [95% CI].

3. **Calibration differences.**
   Compare calibration slope, calibration-in-the-large, or calibration curves.
   - Slope = 1, intercept = 0 indicates perfect calibration.
   - A calibration failure often reflects a different baseline risk at the same
     predictor values. That difference lies in P(Y|X) and counts against
     predictive exchangeability.
   - **Always report alongside discrimination.** Good discrimination with poor
     calibration is the typical pattern when the baseline risk differs at the same
     predictor values.

4. **Coefficient-swap prediction error.**
   Swap estimated coefficients between contexts and measure prediction degradation.
   - Useful for identifying which model components fail to transfer.
   - Applied in neural dynamics analyses (VAR coefficients across experimental runs).

### Small-n Caveats

- Cross-validation estimates of native performance are **unstable** in small
  samples. Prefer leave-one-out or repeated k-fold over single train-test splits.
  Naive CV intervals are too narrow. Take CIs from the nested cross-validation scheme
  of Bates, Hastie and Tibshirani (2024).
- c-statistic has **wide CIs at low event rates**. Apparent preservation of
  discrimination may reflect insufficient power to detect degradation.
- If native performance is estimated on the training data, **overfitting** inflates
  the apparent non-native penalty.
- Single train-test splits produce **one random number**, not a performance estimate.

### Assessment Guidance

**What to ask:**
- Do you have raw data in the target context to evaluate model performance?
- What is the event rate in both contexts? (determines precision of discrimination)
- Has the model been validated externally before? What were the results?
- Are you interested in discrimination (ranking), calibration (absolute predictions),
  or both?

**If predictive similarity condition fails:**
- **Discrimination preserved, calibration fails:** Recalibrate (update intercept /
  baseline hazard). State assumption: "the relative effects of the predictors transfer."
- **Discrimination fails:** Model structure may not transfer. Consider:
  - Updating coefficients (partial refit).
  - Transfer learning (fine-tune on target data).
  - Abandoning transport.
- **Both fail:** The model does not transfer. Refit or find a different approach.

---

## Subcategory 2: Causal Model Similarity

### Similarity Condition

Following Pearl, two conditions can be distinguished:

1. **Structural causal similarity.** Two models are structurally causally similar if
   every endogenous variable has the same parents and the same exogenous causes in
   both (same causal diagram), regardless of effect sizes, functional forms, or
   parameter values.

2. **Effect similarity.** Two models are effect-similar for a target effect if they
   imply the same interventional distribution P(Y | do(X=x)), or the same summary of
   it such as the average treatment effect. Effect similarity depends on both the
   structural functions and the exogenous distributions. Structural causal similarity
   does not imply it.

### Which Form Does the Decision Require?

| Decision | Required form |
|----------|---------------|
| Strict replication | Effect similarity for the target effect |
| Validate a specific causal claim (X→Y) | Same causal diagram apart from the X→Y edge may suffice |
| Transport treatment effect | Structural causal similarity + identifiable selection mechanism |
| Compare effect magnitudes | Effect similarity or explicit transport formula |

Articulating which form the decision requires makes replication claims more precise
and highlights what would constitute a failure to replicate.

### Dissimilarity Functions

1. **Structural Hamming distance (structural causal similarity).**
   The number of edge insertions, deletions and reversals that turn one causal
   diagram into the other.

2. **Parameter distance (effect similarity).**
   For linear SEMs: Frobenius norm ||B_1 - B_2||_F of coefficient-matrix difference.
   For nonlinear models: function-space distances or summary statistics of
   interventional distributions.

3. **Interventional divergence (effect similarity).**
   Compare P(Y | do(X=x)) across models using distributional distances (total
   variation, Wasserstein). Directly quantifies whether the same intervention
   produces the same outcome distribution.

4. **Effect-estimate differences (effect similarity).**
   Compare ATEs, CATEs, or mediation quantities. For transportability, compare
   the target-population effect under each model.
   - Report with CIs: |ATE_A - ATE_B| [95% CI].
   - Wide overlap → consistent with effect similarity.
   - Narrow non-overlap → evidence against effect similarity.

### Small-n Caveats

- Observational data identify a causal diagram **at most up to** the set of diagrams that imply the same conditional independencies. Small samples rarely identify even this set.
  Structural causal similarity claims typically rest on **domain knowledge**, not data-driven
  discovery.
- Effect estimates have **wide CIs** in small n. Difficult to distinguish genuine
  effect heterogeneity from sampling variability.
- Sensitivity analyses for unmeasured confounding are advisable but themselves
  uncertain in small samples.
- DAG estimation algorithms require large n for reliable edge detection.

### Assessment Guidance

**What to ask:**
- What is the proposed causal mechanism? Is there a DAG?
- Is the claim about structure (X causes Y) or magnitude (effect size transfers)?
- What confounders are measured? What might be unmeasured?
- Is structural knowledge from domain expertise or data-driven?

**If causal similarity condition fails** (causal similarity gates only in task 4.3,
Transport causal effect, and is a graded check in tasks 4.1 and 4.2):
- **Structure differs:** The causal claim does not transfer. Different mechanisms
  are at work. Where causal similarity gates (task 4.3), integration is not
  recommended.
- **Structure similar, effects differ:** Effect modification by context-specific
  factors. Can the modifier be identified and adjusted for? This is a modification.
- **Exogenous distributions differ:** Transport formulas may accommodate if the
  selection mechanism is identifiable. This is a modification.
- **Cannot assess:** State as assumption. Sensitivity analysis for structural
  violations.

---

## Subcategory 3: Synthesis Adequacy

### Similarity Condition

A set of studies is **synthesis-adequate** if the distribution of true effects is narrow
enough for the pooled summary to support the stated recommendation.

**When it applies:** A pooled summary of effects is used to support a recommendation,
as in meta-analysis (task 1.2).

### Dissimilarity Functions

1. **Between-study variance and prediction interval.**
   The between-study variance tau^2 and the prediction interval for the effect in a
   new study. I^2 expresses the share of observed variation in excess of chance and
   does not measure the spread of true effects.

### Small-n Caveats

- With few studies, tau^2 is estimated imprecisely. Estimate it by restricted maximum
  likelihood (REML). Report a confidence interval for it (for example Q-profile) and
  the prediction interval rather than tau^2 alone. A near-zero estimate from few studies does not establish
  the condition.
