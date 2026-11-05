# Model Similarity

Model similarity concerns whether the relationships a model encodes — predictions,
effects, causal mechanisms — transfer across contexts. The integration decision
determines which aspect of model behavior matters:

- Transporting a prediction rule → P(Y|X) must be approximately invariant.
- Generalizing a treatment effect → causal structure and effect magnitudes must transfer.

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
  the conditional relationship is preserved.
- **Discrimination and calibration must be reported separately.** Discrimination
  (ranking ability) is a model-subspace property. Calibration (absolute prediction
  accuracy) depends on baseline hazard or prevalence — a data-subspace property.

**The Framingham lesson:**
The Framingham risk score in non-Western populations preserved discrimination
(c-statistic) while calibration failed catastrophically. D'Agostino's recalibration
adjusts S_0(t) (baseline hazard — data subspace) while holding beta fixed (model
subspace). This shows the subspaces are independent: model similarity can coexist
with data dissimilarity.

### Dissimilarity Functions

1. **Cross-application error (non-native penalty).**
   Fit model in context A, evaluate in B (and vice versa). The difference between
   native and non-native performance quantifies the transferability penalty.
   - *Central dissimilarity function for transport decisions.*
   - Report both directions (A→B and B→A) — asymmetry is informative.

2. **Discrimination differences.**
   Compare c-statistic, AUC, or concordance index of the same model in both contexts.
   - c-statistic near 0.5 = no discrimination.
   - Report difference with CI: |AUC_A - AUC_B| [95% CI].

3. **Calibration differences.**
   Compare calibration slope, calibration-in-the-large, or calibration curves.
   - Slope = 1, intercept = 0 indicates perfect calibration.
   - Calibration depends on baseline hazard/prevalence → data-subspace property.
   - **Always report alongside discrimination** — good discrimination + poor
     calibration is the canonical Framingham pattern.

4. **Coefficient-swap prediction error.**
   Swap estimated coefficients between contexts and measure prediction degradation.
   - Useful for identifying which model components fail to transfer.
   - Applied in neural dynamics analyses (VAR coefficients across experimental runs).

### Small-n Caveats

- Cross-validation estimates of non-native performance are **unstable** in small
  samples. Prefer leave-one-out or repeated k-fold with CIs over single
  train-test splits.
- c-statistic has **wide CIs at low event rates**. Apparent preservation of
  discrimination may reflect insufficient power to detect degradation.
- Models fitted on small samples may **overfit** to native data, inflating the
  apparent non-native penalty.
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
  baseline hazard). State assumption: "conditional relationship P(Y|X) transfers."
- **Discrimination fails:** Model structure may not transfer. Consider:
  - Updating coefficients (partial refit).
  - Transfer learning (fine-tune on target data).
  - Abandoning transport.
- **Both fail:** The model does not transfer. Refit or find a different approach.

---

## Subcategory 2: Causal Model Similarity

### Similarity Condition

Following Pearl, causal comparability decomposes into two **independent** conditions:

1. **Structural similarity.** Two models are structurally similar if the same
   variables are causally connected (same DAG), regardless of effect sizes,
   functional forms, or parameter values.

2. **Probabilistic similarity.** Two models are probabilistically similar if the
   distributions of exogenous (unobserved) variables match: P_1(U) = P_2(U).

These conditions are independent:
- Models may share causal structure but differ in exogenous distributions.
- Models may be probabilistically similar yet structurally dissimilar.

**Full causal similarity** = structural + probabilistic similarity. Even under full
similarity, models may differ in effect magnitudes or functional forms.

### Which Form Does the Decision Require?

| Decision | Required form |
|----------|---------------|
| Strict replication | Full causal similarity (structure + probabilistic) |
| Validate a specific causal claim (X→Y) | Structural similarity may suffice |
| Transport treatment effect | Structural similarity + identifiable selection mechanism |
| Compare effect magnitudes | Full causal similarity or explicit transport formula |

Articulating which form the decision requires makes replication claims more precise
and highlights what would constitute a failure to replicate.

### Dissimilarity Functions

1. **Parameter distance.**
   For linear SEMs: Frobenius norm ||B_1 - B_2||_F of coefficient-matrix difference.
   For nonlinear models: function-space distances or summary statistics of
   interventional distributions.

2. **Interventional divergence.**
   Compare P(Y | do(X=x)) across models using distributional distances (total
   variation, Wasserstein). Directly quantifies whether the same intervention
   produces the same outcome distribution.

3. **Effect-estimate differences.**
   Compare ATEs, CATEs, or mediation quantities. For transportability, compare
   the target-population effect under each model.
   - Report with CIs: |ATE_A - ATE_B| [95% CI].
   - Wide overlap → consistent with causal similarity.
   - Narrow non-overlap → evidence of causal dissimilarity.

### Small-n Caveats

- Causal structure **cannot generally be identified** from small observational samples.
  Structural similarity claims typically rest on **domain knowledge**, not data-driven
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

**If causal similarity condition fails:**
- **Structure differs:** The causal claim does not transfer. Different mechanisms
  are at work.
- **Structure similar, effects differ:** Effect modification by context-specific
  factors. Can the modifier be identified and adjusted for?
- **Exogenous distributions differ:** Transport formulas may accommodate if the
  selection mechanism is identifiable.
- **Cannot assess:** State as assumption. Sensitivity analysis for structural
  violations.
