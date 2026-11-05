# Design Similarity

Design similarity concerns the research process: how constructs are operationalized,
how participants are sampled, and what inclusion/exclusion criteria apply. Two studies
may generate very different datasets yet be design-similar if they measure the same
constructs in structurally comparable ways and target the same population. Conversely,
studies with nearly identical data distributions may be design-dissimilar if one uses
self-report while the other uses biomarkers, or if inclusion criteria define
fundamentally different populations.

In the coding-revision case (`05-failure_cases.md`, Case 1), design dissimilarity is
visible before any record is read. When US mortality coding moved from the ninth to the
tenth revision of the International Classification of Diseases (ICD) in 1999, the rules
that assign deaths to cause categories changed, and the codebook documents the change.

Design similarity is often only partially testable — much of the relevant information
resides in protocols, not in observed data.

---

## Subcategory 1: Operationalization Similarity

### Similarity Condition

Two study designs are **operationally equivalent** if they measure the same constructs
on the same measurement scales and with the same measurement-error structure.

**Key distinctions:**
- Operational equivalence requires the same measurement-error **structure and
  magnitude**. Unequal reliability is **not** benign wherever estimates are
  pooled or compared. Unreliable measurement biases a combined estimate toward zero.
  Differences in reliability between sources also create spurious differences between
  their estimates (Wiernik & Dahlke 2020). This second effect grows with the
  attenuation gap.
- It **does** require the same **type** of error. If one study assigns each
  participant the average exposure of a group (Berkson-type error, Armstrong 1998) while
  another measures exposure individually (classical measurement error), they are
  **not** operationally equivalent.

**Examples:**
- Two cohorts measuring systolic BP via automated cuff devices → operationally
  equivalent (even if device brands differ).
- 24-hour ambulatory monitoring vs. single office BP measurement → **not** equivalent
  (different constructs: average daily BP vs. spot BP).

### Dissimilarity Functions

1. **Scale-alignment proportion.**
   For each variable, check whether units, encoding (one-hot vs. ordinal), and
   transformation (log vs. raw) match. Proportion of aligned variables:
   ```
   d_scale = 1 - (1/N) * sum(I{scale_i(x) == scale_i(y)})
   ```
   Values near 0 indicate high alignment.

2. **Measurement-error magnitude differences.**
   Compare standard errors of measurement (the SD of the measurement error) or known
   assay CVs. Their absolute difference provides a continuous dissimilarity function.
   A difference in reliability (McDonald's omega, ICC) also reflects the true-score
   variance of each sample.
   Prefer omega to Cronbach's alpha: alpha assumes tau-equivalent items, so a
   difference in alpha confounds reliability with departure from tau-equivalence.
   Where an ICC is used, state which form.

3. **Harmonization effort.**
   When crosswalks between scoring systems exist, the complexity or residual error
   of the mapping quantifies dissimilarity.

4. **Measurement-invariance tests.**
   Configural, metric, and scalar invariance in factor models. Hierarchical:
   configural (same structure) → metric (same loadings) → scalar (same intercepts).
   These tests return a yes-or-no result, not a distance. Report a continuous effect
   size for non-invariance alongside the test.

### Small-n Caveats

- Measurement-invariance tests require moderate sample sizes for adequate power.
  Non-rejection may reflect low power, not true equivalence.
- When operationalization metadata are incomplete, similarity judgments rest on
  assumptions that cannot be verified from data.
- Reliability estimates (alpha, ICC) are themselves uncertain in small samples;
  confidence intervals may be wide.

### Assessment Guidance

**What to ask the researcher:**
- Do you have study protocols or data dictionaries for both contexts?
- Are outcome definitions identical or do they require mapping?
- What measurement instruments were used? Same device/assay/questionnaire?
- Are there known differences in measurement error type or magnitude?

**Gate failure remediation:**
- If constructs differ: Can a subset of shared constructs support the decision?
- If scales differ: Is a validated crosswalk available?
- If error types differ: Restrict to sources that share the error type. A
  measurement-error model is an analytical adjustment, not a remediation of the gate.

---

## Subcategory 2: Sampling Similarity

### Similarity Condition

Two studies are **sampling-equivalent** if:
1. The unit of sampling is the same (individuals vs. clusters).
2. Inclusion and exclusion criteria target the same population.

**Key distinctions:**
- Sampling equivalence does **not** require identical covariate distributions
  (those are assessed under data similarity).
- It **does** require the same kind of entity sampled under criteria defining the
  same target population.

**Examples:**
- A cluster-randomized trial vs. an individually randomized trial, both targeting
  "adults with hypertension" → can be sampling-equivalent. The unit of randomization
  concerns allocation. Pooling them requires that the cluster-randomized estimate
  accounts for clustering (Cochrane Handbook, chapter 23).
- Two hospital cohorts excluding prior cardiovascular events via comparable
  ICD-code definitions → sampling-equivalent.
- Excluding "known cancer" via self-report vs. via registry linkage → **not**
  sampling-equivalent (inclusion functions differ).

### Dissimilarity Functions

1. **Inclusion-function agreement.**
   Let f_x and f_y map patient characteristics p to inclusion status (0 or 1). If both
   functions are deterministic and known:
   ```
   d_inc = 0 if f_x(p) == f_y(p) for all p, 1 otherwise
   ```
   This binary form gives one differing criterion the same value as wholesale
   divergence, so it cannot rank sources. Applied to one criterion at a time and averaged
   over the criteria, 1 - d_inc gives the *per-criterion eligibility concordance* of `01-decision_catalogue.md`, the share
   of eligibility criteria that agree between sources. Report the denominator used,
   since these proportions are not comparable across papers otherwise.

2. **Propensity-score overlap.** *(cross-subspace proxy)*
   When both samples are drawn from a common super-population, overlap in PS
   distributions quantifies sampling similarity. Metrics: c-statistic of a classifier
   distinguishing samples (near 0.5 = similar), proportion in common support.

3. **Covariate balance summaries.** *(cross-subspace proxy)*
   SMDs on key covariates. |SMD| < 0.1 is conventional but context-dependent.
   Can aggregate as maximum SMD or average SMD. Indices designed for exactly this
   comparison exist and should be preferred to ad hoc summaries where the target
   population is defined: Tipton's generalizability index (2014) and
   propensity-score-based standardized differences (Stuart et al. 2011).

4. **Cluster-structure comparison.**
   For clustered designs: compare number of clusters, cluster sizes, and ICCs.
   The design effect depends on the variable, so report it per variable rather than
   once per design.

### Small-n Caveats

- Inclusion/exclusion criteria may be the primary basis for sampling similarity
  claims, since empirical overlap diagnostics lack power.
- PS models overfit in small samples; overlap diagnostics may be misleading.
- With no true difference, the expected |SMD| is about 0.8 * sqrt(1/n1 + 1/n2)
  (Hedges 1981). With equal sample sizes, it exceeds 0.1 below about 130 units per
  sample. Compare observed SMDs with their permutation distribution.
- With few clusters, cluster-structure assessments have very low precision.

### Cross-Subspace Proxy Warning

PS overlap and covariate balance are computed from **observed data** and strictly
belong to data similarity. They appear here because they are routinely used as
proxies for whether two sampling schemes target the same population.

**This cross-subspace reasoning must be made explicit:** the data-subspace similarity
measure is informative about the design-subspace similarity condition **only** under
the assumption that covariate overlap reflects comparable recruitment, rather than
coincidental balance between populations selected by different criteria.

### Assessment Guidance

**What to ask the researcher:**
- Do you have the full eligibility criteria for both contexts?
- Same unit of analysis (individual, cluster, site)?
- Any known differences in recruitment strategy or referral patterns?
- Can you compute PS overlap from available data?

**Gate failure remediation:**
- If populations differ: Can the analysis be restricted to the intersection?
- If unit of analysis differs: Restrict to sources that sample the same unit.
  Multilevel modeling is an analytical adjustment, not a remediation of the gate.
- If eligibility is unknown: State this as an assumption explicitly.