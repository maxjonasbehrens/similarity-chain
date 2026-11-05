# Data Similarity

Data similarity concerns the joint distribution of observed variables, conditional on
a fixed operationalization and sampling scheme. While design similarity asks whether
studies *could* generate comparable data, data similarity asks whether the data they
*did* generate are distributionally close.

Data similarity concerns observed (empirical) distributions, not the data-generating
mechanism. Two datasets may have similar empirical distributions despite arising from
different causal processes; conversely, the same mechanism may produce dissimilar
samples due to finite-sample variation.

**Two roles in the protocol:**
1. **Precondition for model similarity**: If covariate distributions have no overlap,
   prediction-model transport is ill-defined.
2. **Endpoint in its own right**: When the decision is whether to pool datasets or
   treat distributional differences as heterogeneity.

**Guiding principle (empirical/assumption boundary):** Dissimilarity functions should
quantify properties directly observable in the data. When a dissimilarity function requires
a modeling assumption (e.g., same distributional family, MAR missingness), that
assumption belongs in the assumption inventory, not buried in the choice of function.

---

## Subcategory 1: Signature Similarity

### Similarity Condition

Two datasets are **signature-equivalent** if their variables can be placed in
one-to-one correspondence with matching data types, encodings, units of analysis,
and time aggregation.

**Key distinctions:**
- Signature equivalence implies any recoding (one-hot vs. label) is invertible and
  the unit of observation (patient-level vs. visit-level) matches.
- It does **not** imply equality of values or distributions.

**Examples:**
- Two datasets with columns `age`, `sex`, `bmi` at baseline, all numeric → equivalent.
- Dataset with `bmi` averaged over follow-up vs. baseline `bmi` → **not** equivalent.

### Dissimilarity Functions

1. **Schema-match proportion.**
   Fraction of variables with matching names, types, and units.

2. **Alignment cost.**
   If harmonization is required, the number of transformations (recoding, rescaling,
   aggregation) needed to align schemas.

**Retention and purity, where a correspondence can be observed directly.**
Where a provider has coded the same records under both schemes (a dual-coded file, a
bridge-coding study, a period of parallel running), the correspondence is observable
rather than assumed. Report two quantities per category instead of one. *Retention* is
the share of a category's records under the old scheme that remain in it under the new
one. *Purity* is the share of its records under the new scheme that were in it under
the old one. A single ratio of totals cannot separate the two mechanisms: a category
can shrink because its own records leave, or grow because records arrive from
elsewhere. A category in which both exceed a stated threshold may be carried across the
break. A category failing either should be reported as discontinuous rather than
corrected (`05-failure_cases.md`, Case 1).

**Strict bijection is a poor guide to remediation.** Signature equivalence asks for a
one-to-one correspondence between variables. Within a coded variable, strict bijection
between categories (each category maps to exactly one category and is reached from
exactly one) is a poorer guide than it appears. A single misassigned record
disqualifies a category that is otherwise sound, and coarsening the classification to
repair it can remove the one-to-one correspondence entirely.

### Small-n Caveats

- Signature assessments are metadata-based and **do not depend on sample size**.
- However, incomplete documentation in small studies may make equivalence unverifiable.

### Assessment Guidance

**What to ask:**
- Do you have data dictionaries for both datasets?
- Are variable names standardized (e.g., CDISC, OMOP)?
- Same unit of observation? Same time point definitions?
- Any variables requiring transformation to align?

**Gate failure remediation:**
- If some variables cannot be placed in correspondence: Can the variables that can
  support the task?
- If categories of a coded variable changed between schemes: Carry across only the
  categories whose correspondence is verified (retention and purity both above a
  stated threshold), and report the others as discontinuous. Rescaling by a ratio of
  totals is not a remediation, because it cannot separate records that left a
  category from records that arrived.

---

## Subcategory 2: Distributional Similarity

### Similarity Condition

Two datasets are **distributionally comparable** if their variables have been placed
in correspondence (signature equivalence holds) and are measured on scales that permit
quantitative comparison.

This is a minimal condition: computing a distance between empirical distributions is
meaningful without requiring that both come from the same parametric family.

**When a parametric dissimilarity function is used** (e.g., comparing fitted Gaussian
parameters), the additional assumption of distributional-family equivalence must be
stated explicitly.

**Examples:**
- Two cohorts with overlapping covariate ranges and comparable outcome rates →
  comparable regardless of distribution shape.
- Zero-inflated counts vs. Poisson counts → comparable (empirical distributions can
  be compared), but claiming distributional-family equivalence requires an assumption.

### Dissimilarity Functions

1. **Nonparametric distributional distances.**
   Energy distance, maximum mean discrepancy (MMD), Wasserstein distance, KS statistics.
   No distributional-family assumption required.
   - *When to use:* Default choice. Works for any variable type.
   - *How to interpret:* Compare against permutation null distribution.
   - *Caution:* Energy distance is MMD computed with the distance kernel. Report one
     of them, not both as if they were independent checks.
   - *Caution:* When n is small relative to the number of variables p (n = o(sqrt(p))),
     both statistics retain power only against differences in means and in the trace
     of the covariance. Report the n-to-p ratio alongside the statistic, and read a
     small distance in a small, wide sample as uninformative rather than reassuring.

2. **Standardized mean differences (SMDs).**
   Per-covariate scale-free location summary. |SMD| < 0.1 is conventional.
   - *When to use:* Quick balance diagnostic; widely understood.
   - *Limitation:* Only captures location shift, not shape differences.

3. **Prognostic-score balance.**
   Balance on a score that predicts the outcome from covariates, fitted in one source
   and applied to both.
   - *When to use:* When balance measures disagree. It is the only balance diagnostic
     that targets bias in the estimand directly rather than covariate discrepancy as a
     proxy.

4. **Density-ratio diagnostics.**
   c-statistic of a classifier trained to distinguish samples. Values near 0.5
   indicate high similarity.
   - *When to use:* Captures multivariate distributional differences.
   - *Limitation:* Overfits in small samples; use permutation null.

5. **Parametric distances.**
   Differences in fitted parameters (|mu1 - mu2|, ||Sigma1 - Sigma2||_F).
   Valid only when distributional-family assumption holds.
   - *When to use:* Only when parametric family is justified.
   - *State assumption:* "Both datasets follow [family]."

### Small-n Caveats

- Distributional dissimilarity functions have high variance in small samples.
  **Bootstrap confidence intervals are essential.**
- Classifier-based measures overfit; **permutation-based null distributions** advisable.
- Model-selection tests for distributional form have low power: failure to reject
  does not confirm the assumed family.

### Assessment Guidance

**What to ask:**
- Do you have raw data from both contexts for empirical comparison?
- Which covariates are most important for the decision?
- Are there known population differences (age, comorbidity burden, geography)?
- What outcome rates are expected? (affects power of distributional tests)

**If not met (distributional similarity is a graded check in every cataloged task
except the network task, so these are modifications):**
- If distributions don't overlap: Restrict to common support.
- If outcome rates differ markedly: Recalibration may address prevalence differences
  but requires stating the assumption that the model relationship holds.
- If only summary data available: Report known differences; flag empirical
  dissimilarity functions as infeasible.

---

## Subcategory 3: Dependence Similarity

### Similarity Condition

Two datasets are **dependence-comparable** if their variables have been placed in
correspondence and pairwise associations can be meaningfully computed.

The empirical content: rank-based association measures and observed
conditional-independence patterns are similar across datasets.

**Key distinctions:**
- Does **not** require equality of marginal distributions. Dependence structure can
  be compared separately from marginal shape.
- Concerns observable associations (correlations, conditional independence), **not**
  causal dependence — whether causal relationships transfer is a model-subspace
  question.

**Examples:**
- Two datasets where age-BP association follows the same monotone pattern →
  dependence-similar (even if one cohort is older).
- Linear age-BP association in one, U-shaped in another → **not** dependence-similar.

### Dissimilarity Functions

1. **Equality of dependence structure.**
   Use a test built for the question: equality of two correlation matrices (Jennrich),
   equality of two copulas (Rémillard and Scaillet), or a permutation-based network
   comparison test for graphical structures. Pairwise differences |tau1 - tau2| or
   |rho1 - rho2| help locate *where* structures differ once a test has shown *that*
   they differ. Aggregating them is no substitute for a test with a known null
   distribution.

2. **Conditional-independence comparison.**
   Compare partial correlations or graphical-model edge sets.

3. **Mutual-information differences.**
   Compare estimated MI between variable pairs. Distance correlation is a natural
   companion to energy distance, since it detects any dependence.

4. **Copula-parameter distance.**
   If a parametric copula is fitted (stated as assumption), compare parameters.

### Small-n Caveats

- Rank correlations are relatively robust but have non-trivial variance in small
  samples; report CIs.
- Graphical-model estimation is highly unstable in small n; edge recovery has low
  power and high false-discovery rates.
- With few observations, prefer pairwise rank correlations over copula estimation.

### Assessment Guidance

**What to ask:**
- Are there known interaction effects or non-linear relationships in the data?
- Can you compute correlation matrices from both datasets?
- Is the decision sensitive to association patterns (e.g., moderation, interaction)?

---

## Subcategory 4: Missingness Similarity

### Similarity Condition

Two datasets are **missingness-comparable** if their missingness patterns can be
described using the same observable summaries: proportion missing per variable,
pattern frequencies, and associations between missingness indicators and covariates.

**Key distinctions:**
- Claims about the underlying mechanism (MAR, MNAR, limit of detection) are
  **untestable assumptions** and belong in the assumption inventory.
- The empirical similarity condition asserts that the *observable signatures* of
  missingness are similar, without committing to a mechanism.

**Examples:**
- Two cohorts with similar missingness rates in lab values and similar
  associations between missingness indicators and covariates → comparable.
- 40% outcome missingness due to death vs. 5% administrative missingness →
  **not** comparable (observable patterns differ sharply).

### Dissimilarity Functions

1. **Systematically missing variables.**
   A variable recorded in one source and wholly absent from another. This is a
   structural feature of the combination, not a difference in degree, and a
   missingness rate cannot express it. Classify every variable as present in all
   sources, sporadically missing within sources, or systematically missing from at
   least one, and report the third category separately. Multilevel and study-level
   imputation methods from individual-participant-data meta-analysis address it. A
   variable that is constant and unrecorded in one source (a treatment given to every
   participant and therefore never coded) is systematically missing and also
   confounded with source.

2. **Missingness-rate comparison.**
   Differences in proportion missing per variable. Directly observable.

3. **Missingness-pattern comparison.**
   Compare distribution of missingness patterns (pattern frequencies, classifier
   distinguishing datasets by missingness vectors).

4. **Auxiliary-variable association.**
   Compare strength of association between missingness indicators and observed
   covariates. Similar associations provide indirect evidence about mechanism.

5. **Sensitivity-analysis concordance.**
   If sensitivity analyses under alternative mechanisms (MNAR) are conducted,
   compare the range of results.

### Small-n Caveats

- Pattern tables become sparse in small n; many patterns have zero or few observations.
- Imputation models are less stable in small samples.
- MAR and MNAR cannot be told apart from observed data: for every MNAR model there is
  an MAR model that fits the observed data equally well. Sensitivity analysis under
  stated departures is the only available response.

### Assessment Guidance

**What to ask:**
- What proportion of data is missing in each context?
- Is missingness likely related to the outcome (informative missingness)?
- Same data collection infrastructure? (EMR vs. manual entry vs. registry)
- Were the same variables collected in both contexts?
- Is any variable recorded in one source and absent from another?

**If not met (missingness similarity is never a gate, so these are modifications):**
- If mechanisms likely differ: Sensitivity analyses under different assumptions.
- If rates differ dramatically: May indicate fundamentally different populations
  or data collection processes — reassess design similarity.

---

## Subcategory 5: Evidence Compatibility

### Similarity Condition

A set of studies is **evidence-compatible** if study and population attributes that the
search strategy does not fix do not systematically explain differences in observed
effects.

**When it applies:** The sources are studies whose effect estimates are combined, as in
meta-analysis (task 1.2).

### Dissimilarity Functions

1. **Subgroup analysis and meta-regression.**
   Relate the effect estimates to study attributes.

2. **Funnel plot asymmetry.**
   An association between effect size and precision can indicate that the studies
   found differ from the studies conducted.

### Small-n Caveats

- With few studies, subgroup analyses and meta-regression have low power. The absence
  of an association does not establish the condition.
