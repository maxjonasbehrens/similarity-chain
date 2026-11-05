# Task Catalog

20 analytical tasks organized in 6 families. Families and tasks follow the order of the task catalog in the supplement of the preprint. The task numbers (1.1, 1.2, …) are this skill's own. The four links of the chain are **analytical task → subspace → condition → dissimilarity function**.

## How to use this catalog

Each entry gives the required subspaces, their assessment order with the gates marked, the reason for each gate, and representative dissimilarity functions. The conditions for each subspace are in `02-design_similarity.md`, `03-data_similarity.md` and `04-model_similarity.md`.

1. Match the researcher's analytical task to the closest entry (use the "Choosing between adjacent tasks" table for near misses).
2. Read the required subspaces and the assessment order. Gates are marked.
3. Assess each subspace in order, using the condition and dissimilarity-function references.
4. A gate that is not met means downstream assessments are not meaningful unless a structural remediation resolves it. Without one, integration is not recommended.

## Notes on gate assignment

Families 1, 3, 4, 5 and 6 concern *combined-data* integration, in which two or more sources are used together within a single analysis. Family 2 concerns applying a model fitted in one context to another, as does *Transport causal effect* in Family 4. These are included because the same ordering logic extends to them, but they are not the setting the paper addresses, and the empirical work reported in the paper does not cover them.

Four dissimilarity functions below carry this framework's own names. The *scale-alignment proportion* is the share of variables whose units, coding and transformation match. It relates to harmonization potential as defined by Fortier et al. (2017). The *schema-match proportion* is the share of variables with matching names, types and units. The schema-matching literature evaluates such matches by precision, recall and F1 against a stated gold standard. The *key-agreement proportion* is the share of linkage keys (the identifiers used to join records) whose coverage, format and definition agree across sources. The record-linkage literature reports false-match and missed-match rates alongside it. *Per-criterion eligibility concordance* is the share of eligibility criteria that agree between sources. It applies inclusion-function agreement (see `02-design_similarity.md`) to one criterion at a time. Report the denominator of each proportion, since otherwise the proportions are not comparable across papers. Where the estimand is an association pooled across sources with non-identical instruments, moderated nonlinear factor analysis offers a model-based alternative to the scale-alignment proportion.

Distributional similarity is a graded check in every task below that includes it, with one exception. Missingness and predictive similarity are never gates. A distributional difference moves an estimate, and weighting or adjustment can often address it. But weighting needs information to weight with. In *Compare treatments through a network of trials*, aggregate data do not provide it, so distributional similarity gates there. Dependence gates only in *Fuse non-overlapping sources*, where the estimate rests on an untestable conditional independence. Both exceptions follow the rule of the paper's gate logic: a subspace is a gate when its condition fixes what the downstream quantities refer to. The gate assignments therefore describe these twenty tasks, not the subspaces. In small area estimation (model-based estimates for domains with almost no direct observations), for example, the estimates for those domains extrapolate a model beyond the data it was fitted on, so a predictive condition gates.

Most conditions concern two sources at a time, so no pairwise dissimilarity function can detect a condition that holds only for a whole set of sources. The network task is such a case. In *Meta-analyze*, evidence compatibility and synthesis adequacy are set-level conditions, and their dissimilarity functions take all studies at once. Family 6 names a task that the subspaces do not describe at all.


## Family 1: Combine data

### 1.1 Pool cohorts

| Field | Value |
|-------|-------|
| **Analytical task** | Merge individual-level data from two or more cohorts for joint analysis |
| **Required subspaces** | Design: operationalization → Data: signature → Data: distributional → Model: predictive |
| **Assessment order** | 1. Operationalization (gate) → 2. Signature (gate) → 3. Distributional → 4. Predictive |
| **Gate logic** | If constructs or scales do not match, pooling produces artifacts. If variables cannot be placed in correspondence, merging is impossible. The model subspace is a graded check: where the predictor–outcome relationship differs across sources, a pooled fit still estimates something, namely an average of them, and that average answers the intended question if it was the estimand. It is required nonetheless, because the distributional check does not settle it. Coefficients can differ across sources for two reasons. Case-mix may differ, so that linear terms approximate one nonlinear relationship over different covariate regions. Or comparable individuals may relate differently to the outcome. Only the second reason bears on whether a single pooled relationship is meaningful |
| **Dissimilarity functions** | Scale-alignment proportion; schema-match proportion; energy distance, which corresponds to the maximum mean discrepancy (MMD) with the distance kernel, and standardized mean differences (SMDs), with confidence intervals and the ratio of sample size to number of covariates (n-to-p) reported; source-specific coefficients against a pooled fit, and the case-mix versus contextual decomposition that separates the two reasons for that difference |

### 1.2 Meta-analyze

| Field | Value |
|-------|-------|
| **Analytical task** | Combine effect estimates across studies (aggregate-level) |
| **Required subspaces** | Design: operationalization → Data: evidence compatibility → Model: synthesis adequacy |
| **Assessment order** | 1. Operationalization (gate) → 2. Evidence compatibility → 3. Synthesis adequacy |
| **Gate logic** | If populations, interventions, comparators, outcomes or study designs are defined differently, effects are not comparable. Study attributes that explain differences in effects, or a wide distribution of true effects, limit what the pooled estimate supports. Eligibility here means the review's criteria for including trials, so it is part of this gate and not of sampling equivalence. |
| **Dissimilarity functions** | Search strategy applied at screening; per-criterion eligibility concordance (a share over the criteria, not one yes or no); scale-alignment proportion of the outcome measures; subgroup analyses, meta-regression on study attributes and funnel plot asymmetry; between-study variance τ² with its prediction interval |

### 1.3 Compare treatments through a network of trials

| Field | Value |
|-------|-------|
| **Analytical task** | Estimate relative effects among treatments that have not all been compared head to head, combining direct and indirect evidence across a connected network |
| **Required subspaces** | Design: operationalization → Design: sampling → Data: distributional |
| **Assessment order** | 1. Operationalization (gate) → 2. Sampling (gate) → 3. Distributional (gate) |
| **Gate logic** | As in *Meta-analyze*, differing outcome definitions make effects incomparable. Sampling gates because an indirect contrast built from trial sets with different eligibility refers to no single population. Beyond that, this is the one task in the catalog where *distributional* similarity is itself a gate. An effect modifier distributed differently across the sets of trials contributing each treatment biases the indirect contrast, and with aggregate data there is nothing to weight with, so the imbalance is not a matter of degree but the reason the contrast is not identified. The condition is a property of a *set* of trials and of which contrast each contributes, not of any pair, so no pairwise dissimilarity function between two studies detects its failure. Its statistical counterpart, consistency (agreement of direct and indirect estimates of one contrast), can be checked only where the network contains a closed loop. In a star network, every trial compares an active treatment with the same common comparator, so no sample size makes consistency testable |
| **Dissimilarity functions** | Covariate SMDs computed between the trial *sets* contributing each contrast rather than between individual trials; per-criterion eligibility concordance; scale-alignment proportion. Statistical consistency diagnostics (node-splitting, design-by-treatment interaction) assess the same condition after the fact and only within closed loops, and should be reported alongside the network geometry that determines whether they can be applied at all |

### 1.4 Federated analysis

| Field | Value |
|-------|-------|
| **Analytical task** | Analyze distributed data without centralizing (each site runs local computation) |
| **Required subspaces** | Design: operationalization → Data: signature → Data: distributional → Data: missingness → Model: predictive |
| **Assessment order** | 1. Operationalization (gate) → 2. Signature (gate) → 3. Distributional → 4. Missingness → 5. Predictive |
| **Gate logic** | The gates and checks are those of *Pool cohorts*, including the model subspace, since one model is fitted across the sites whether or not the computation is centralized. Missingness patterns should also be comparable, since each site imputes locally. |
| **Dissimilarity functions** | Scale-alignment proportion; schema-match proportion; energy distance; systematically-missing-variable inventory; site-specific coefficients against the federated fit |

### 1.5 Splice a series across a regime change

| Field | Value |
|-------|-------|
| **Analytical task** | Chain data collected under different definitional, coding or instrument regimes into one continuous series |
| **Required subspaces** | Design: operationalization → Data: signature → Data: distributional |
| **Assessment order** | 1. Operationalization (gate) → 2. Signature (gate) → 3. Distributional |
| **Gate logic** | If the construct or its coding changes at the boundary, apparent change over the series confounds definitional revision with real change. If the categories on either side cannot be placed in correspondence, no rescaling restores comparability. |
| **Dissimilarity functions** | Scale-alignment proportion; schema-match proportion; and, where the provider supplies records coded under both regimes, comparability ratios (counts under the new coding divided by counts under the old) and *retention* (the share of a category's records under the old coding that stay in it under the new) and *purity* (the share of its records under the new coding that were in it under the old) per category, which separate records leaving a category from records arriving from elsewhere |

### 1.6 Fuse non-overlapping sources

| Field | Value |
|-------|-------|
| **Analytical task** | Combine sources that share covariates but never jointly observe the two quantities whose association is of interest (statistical matching, data fusion) |
| **Required subspaces** | Data: signature → Data: dependence → Data: distributional |
| **Assessment order** | 1. Signature (gate) → 2. Dependence (gate) → 3. Distributional |
| **Gate logic** | The shared covariate block carries the entire join, so its variables must correspond exactly. The two never-jointly-observed quantities are then assumed conditionally independent given that block. That assumption is untestable from the fused data and is what the estimate rests on: if it fails, the reported association is an artifact of the assumption rather than an estimate of anything. This is the one task in the catalog where dependence gates. |
| **Dissimilarity functions** | Schema-match proportion on the shared block; Fréchet-bound width by conditioning set, which quantifies how far the shared covariates identify the unobserved association; sensitivity of the fused association to pre-specified departures from conditional independence |

### 1.7 Link records across sources

| Field | Value |
|-------|-------|
| **Analytical task** | Join two or more sources at the level of individual entities, deterministically or probabilistically, before any analysis |
| **Required subspaces** | Data: signature → Design: sampling → Data: missingness |
| **Assessment order** | 1. Signature (gate) → 2. Sampling → 3. Missingness |
| **Gate logic** | Linkage keys must identify the same entities in both sources. If identifiers differ in coverage, format or definition, linked and unlinked records differ systematically. Match rates that vary by subgroup then act as a selection mechanism on every downstream analysis, whether or not linkage was the object of interest. |
| **Dissimilarity functions** | Key-agreement proportion; false-match and missed-match rates by subgroup (a raw match rate confounds true non-overlap with linkage failure and is silent on false matches); comparison of linked against unlinked records on observed covariates |

### 1.8 Estimate a population quantity from complex samples

| Field | Value |
|-------|-------|
| **Analytical task** | Pool two or more probability samples (survey cycles, waves, rounds) to estimate a population prevalence, mean or total, and its change over time |
| **Required subspaces** | Design: sampling → Design: operationalization → Data: signature → Data: distributional |
| **Assessment order** | 1. Sampling (gate) → 2. Operationalization (gate) → 3. Signature (gate) → 4. Distributional |
| **Gate logic** | The sampling design fixes which population an estimate refers to, so sampling gates first. What a sampling incompatibility destroys depends on the estimand. If the provider reweights every round to population control totals, a whole-population estimand survives a change of oversampling domains. An estimand defined on a domain whose definition or oversampling changed does not: precision shifts discontinuously at the boundary, and a series fitted across it reads the design change as signal. Weight bases that differ by a constant factor distort totals but not proportions, since the factor cancels in a ratio. A differently constructed weight distorts both. Operationalization gates as in *Pool cohorts*. Signature gates because a renamed or recoded survey variable may no longer refer to the same quantity. |
| **Dissimilarity functions** | Per-criterion eligibility concordance; frame and stratum comparability (a checklist, not a scalar); design effect and effective n, reported per variable rather than per design, and paired with Kish's 1 + CV²(w) (the design effect from unequal weights w, with CV their coefficient of variation) because a linearized design effect can rest on few strata and is then unstable; effective n within each reporting domain, compared across rounds; scale-alignment proportion; schema-match proportion |

## Choosing between adjacent tasks

Several entries describe operations that look alike. The discriminator is almost always the estimand, or the role each source plays, rather than the mechanics of the merge.

| If... | then |
|---|---|
| the sources are sequential eras of one system, and the boundary lies along the axis being reported | *Splice a series*, not *Pool cohorts* |
| the estimand is a design-based population quantity | *Estimate a population quantity*, not *Pool cohorts* or *Splice a series*. A covariate-adjusted association stays with those tasks. |
| one source is the study and the other supplies a comparator or control arm | *Borrow external controls*, not *Pool cohorts* |
| the sources are deliberately kept separate, one to fit and one to assess | *Externally validate* (extension), not *Pool cohorts* |
| the sources never jointly observe the two quantities whose association is wanted | *Fuse non-overlapping sources* |
| the treatments of interest were never compared head to head, and the estimate rests on a common comparator | *Compare treatments through a network of trials*, not *Meta-analyze* |
| the sources differ by measurement platform rather than by population | *Integrate an omics layer across platforms*, not *Pool cohorts* |
| entities are joined individually before any analysis | *Link records*, usually a precondition for pooling rather than an alternative to it |

Pooling sources only to enlarge a benchmark for comparing methods is not a cataloged task. Its gates are those of the task the methods are meant to perform, and a benchmark study should name that task.


## Family 2: Use model from elsewhere

*Extension family, outside the combined-data scope of the paper (see Notes on gate assignment).*

### 2.1 Transport prediction model

| Field | Value |
|-------|-------|
| **Analytical task** | Apply a prediction model trained in context A to make predictions in context B |
| **Required subspaces** | Design: operationalization → Data: distributional → Model: predictive |
| **Assessment order** | 1. Operationalization (gate) → 2. Distributional → 3. Predictive |
| **Gate logic** | If outcome definitions differ, predictions are for different targets. Distributional differences move performance, so distributional similarity is a graded check here. |
| **Dissimilarity functions** | Scale-alignment proportion; energy distance / covariate SMDs; cross-application area under the receiver operating characteristic curve (AUC), calibration slope and intercept |

### 2.2 Externally validate

| Field | Value |
|-------|-------|
| **Analytical task** | Assess whether a model's performance holds in a new context (without retraining) |
| **Required subspaces** | Design: operationalization → Data: distributional → Model: predictive |
| **Assessment order** | 1. Operationalization (gate) → 2. Distributional → 3. Predictive |
| **Gate logic** | As for *Transport prediction model*. Validation is transport evaluated rather than applied. |
| **Dissimilarity functions** | As for *Transport prediction model*. Report discrimination and calibration separately. |

### 2.3 Calibrate / recalibrate

| Field | Value |
|-------|-------|
| **Analytical task** | Adjust a model's predictions to a new context (update intercept/baseline, keep slopes) |
| **Required subspaces** | Design: operationalization → Model: predictive → Data: distributional |
| **Assessment order** | 1. Operationalization (gate) → 2. Predictive (partial: discrimination only) → 3. Distributional |
| **Gate logic** | Recalibration assumes the model's ranking (discrimination) transfers even if absolute predictions do not. |
| **Dissimilarity functions** | Scale-alignment proportion; discrimination comparison; baseline hazard / prevalence comparison |

### 2.4 Transfer learn

| Field | Value |
|-------|-------|
| **Analytical task** | Use a pre-trained model as a starting point and fine-tune it on target data |
| **Required subspaces** | Data: signature → Data: distributional → Model: predictive |
| **Assessment order** | 1. Signature (gate) → 2. Distributional → 3. Predictive |
| **Gate logic** | Input features should correspond. Distributional overlap affects how much fine-tuning is needed. |
| **Dissimilarity functions** | Schema-match proportion; energy distance; performance gap between the model without and with fine-tuning |


## Family 3: Borrow information

### 3.1 Borrow external controls

| Field | Value |
|-------|-------|
| **Analytical task** | Use control-arm data from another study or historical database |
| **Required subspaces** | Design: sampling → Data: distributional → Data: missingness |
| **Assessment order** | 1. Sampling (gate) → 2. Distributional → 3. Missingness |
| **Gate logic** | If eligibility criteria differ, the external *controls* represent a different population. |
| **Dissimilarity functions** | Per-criterion eligibility concordance; propensity-score overlap with an explicit positivity diagnostic; systematically-missing-variable inventory |

### 3.2 Impute from reference

| Field | Value |
|-------|-------|
| **Analytical task** | Use a reference dataset to impute missing variables in the target |
| **Required subspaces** | Data: signature → Data: distributional → Data: dependence → Data: missingness |
| **Assessment order** | 1. Signature (gate) → 2. Distributional → 3. Dependence → 4. Missingness |
| **Gate logic** | Variables should correspond. Imputation requires distributional and dependence similarity, both graded checks here. Dependence is graded because the target observes the imputed variables in part, so a difference in dependence moves the imputed values and can be checked. A variable that is systematically missing in the target is the situation of *Fuse non-overlapping sources*, where dependence gates. |
| **Dissimilarity functions** | Schema-match proportion; energy distance; a test for equality of dependence structure (copula-equality or correlation-matrix equality, not differenced rank correlations); systematically-missing-variable inventory |

### 3.3 Annotate with reference atlas

| Field | Value |
|-------|-------|
| **Analytical task** | Map new observations to categories/labels defined in a reference dataset |
| **Required subspaces** | Data: signature → Data: distributional |
| **Assessment order** | 1. Signature (gate) → 2. Distributional |
| **Gate logic** | Variables should be in one-to-one correspondence before any distributional comparison. |
| **Dissimilarity functions** | Schema-match proportion; energy distance (equivalently MMD with the distance kernel) |


## Family 4: Compare / reproduce

### 4.1 Replicate finding

| Field | Value |
|-------|-------|
| **Analytical task** | Test whether a reported result reproduces in a new context |
| **Required subspaces** | Design: operationalization → Design: sampling → Model: causal |
| **Assessment order** | 1. Operationalization (gate) → 2. Sampling → 3. Causal |
| **Gate logic** | If constructs are measured differently, the *replication* tests a different claim. |
| **Dissimilarity functions** | Scale-alignment proportion; per-criterion eligibility concordance; effect-estimate differences with confidence intervals |

### 4.2 Emulate a trial with real-world evidence

| Field | Value |
|-------|-------|
| **Analytical task** | Use real-world data to emulate a randomized trial, where a trial is infeasible or where the evidence is intended to complement one |
| **Required subspaces** | Design: sampling → Design: operationalization → Model: causal |
| **Assessment order** | 1. Sampling (gate) → 2. Operationalization (gate) → 3. Causal |
| **Gate logic** | Eligibility differences imply different populations. Outcome definition differences imply different endpoints. |
| **Dissimilarity functions** | Per-criterion eligibility concordance; scale-alignment proportion; effect-estimate differences with confidence intervals |

### 4.3 Transport causal effect

*Extension task (see Notes on gate assignment).*

| Field | Value |
|-------|-------|
| **Analytical task** | Generalize a treatment effect estimated in one population to a different target population |
| **Required subspaces** | Model: causal → Design: sampling → Data: distributional |
| **Assessment order** | 1. Causal (gate) → 2. Sampling → 3. Distributional |
| **Gate logic** | Causal structure should transfer for the effect to be meaningful in the target. Differences in sampling and covariate distributions are what transport corrects for, so they are graded checks. |
| **Dissimilarity functions** | Propensity-score overlap with a positivity diagnostic; per-criterion eligibility concordance; covariate SMDs |


## Family 5: Align measurement

### 5.1 Harmonize instruments

| Field | Value |
|-------|-------|
| **Analytical task** | Create comparable measures from instruments that differ across contexts |
| **Required subspaces** | Design: operationalization |
| **Assessment order** | 1. Operationalization (gate) |
| **Gate logic** | This *is* the gate. Harmonization establishes operational equivalence. |
| **Dissimilarity functions** | Scale-alignment proportion; reliability differences; measurement-invariance tests |


## Family 6: Integrate omics layers

The name *multi-omics integration* covers two operations, and the chain covers one of them. The *horizontal* case combines the same assay measured in several cohorts or on several platforms and is cataloged below. The *vertical* case combines different assays (transcriptome, methylome, proteome) measured on the same specimens and asks what structure they share. The subspaces do not apply to it, because they compare two contexts that observe the same construct. RNA-seq and methylation are not meant to measure the same thing. Scored anyway, the pair would get a scale-alignment proportion near zero, and the chain would return Integration not recommended for a standard, valid analysis. Vertical integration needs subspaces this framework does not define: whether the modalities share specimens, whether their latent structure is shared or modality-specific, and whether one layer dominates a joint decomposition through its dimension or noise scale. We name the task without a catalog entry, since an entry would imply coverage the chain does not have.

### 6.1 Integrate an omics layer across platforms

| Field | Value |
|-------|-------|
| **Analytical task** | Combine the same assay measured in two or more cohorts, laboratories or platforms into one analysis |
| **Required subspaces** | Data: signature → Design: operationalization → Data: distributional |
| **Assessment order** | 1. Signature (gate) → 2. Operationalization (gate) → 3. Distributional |
| **Gate logic** | Signature leads here, reversing the order used for *Pool cohorts*, because features are identified by platform-specific probes or annotations and a failure to place them in correspondence cannot be repaired downstream. Operationalization then asks whether the same biological quantity is being reported, which assay generation and normalization pipeline can change without changing the feature name. Distributional similarity is a graded check, but its remedy can overreach: batch correction that removes genuine biological signal along with the batch effect is the characteristic failure, so the correction should be assessed against a criterion that separates the two rather than by whether the batches visually merge |
| **Dissimilarity functions** | Schema-match proportion over feature identifiers, with the annotation build stated; scale-alignment proportion across assay generations; batch-mixing and biological-conservation metrics reported as a pair rather than singly |

## Quick reference: task → subspaces → gates

| Task | Assessment order |
|---|---|
| 1.1 Pool cohorts | 1. Operationalization (gate) → 2. Signature (gate) → 3. Distributional → 4. Predictive |
| 1.2 Meta-analyze | 1. Operationalization (gate) → 2. Evidence compatibility → 3. Synthesis adequacy |
| 1.3 Compare treatments through a network of trials | 1. Operationalization (gate) → 2. Sampling (gate) → 3. Distributional (gate) |
| 1.4 Federated analysis | 1. Operationalization (gate) → 2. Signature (gate) → 3. Distributional → 4. Missingness → 5. Predictive |
| 1.5 Splice a series across a regime change | 1. Operationalization (gate) → 2. Signature (gate) → 3. Distributional |
| 1.6 Fuse non-overlapping sources | 1. Signature (gate) → 2. Dependence (gate) → 3. Distributional |
| 1.7 Link records across sources | 1. Signature (gate) → 2. Sampling → 3. Missingness |
| 1.8 Estimate a population quantity from complex samples | 1. Sampling (gate) → 2. Operationalization (gate) → 3. Signature (gate) → 4. Distributional |
| 2.1 Transport prediction model | 1. Operationalization (gate) → 2. Distributional → 3. Predictive |
| 2.2 Externally validate | 1. Operationalization (gate) → 2. Distributional → 3. Predictive |
| 2.3 Calibrate / recalibrate | 1. Operationalization (gate) → 2. Predictive (partial: discrimination only) → 3. Distributional |
| 2.4 Transfer learn | 1. Signature (gate) → 2. Distributional → 3. Predictive |
| 3.1 Borrow external controls | 1. Sampling (gate) → 2. Distributional → 3. Missingness |
| 3.2 Impute from reference | 1. Signature (gate) → 2. Distributional → 3. Dependence → 4. Missingness |
| 3.3 Annotate with reference atlas | 1. Signature (gate) → 2. Distributional |
| 4.1 Replicate finding | 1. Operationalization (gate) → 2. Sampling → 3. Causal |
| 4.2 Emulate a trial with real-world evidence | 1. Sampling (gate) → 2. Operationalization (gate) → 3. Causal |
| 4.3 Transport causal effect | 1. Causal (gate) → 2. Sampling → 3. Distributional |
| 5.1 Harmonize instruments | 1. Operationalization (gate) |
| 6.1 Integrate an omics layer across platforms | 1. Signature (gate) → 2. Operationalization (gate) → 3. Distributional |
