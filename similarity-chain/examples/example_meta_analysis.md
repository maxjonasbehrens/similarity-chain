# Worked Example: Pooling Mortality Risk Ratios Across Clozapine Trials

> **Scope note.** This is the running example of the paper (Section 3, "Working
> Through the Chain"). Meta-analysis is a combined-data task, inside the scope of the
> paper. Every fact below comes from the paper. Where the protocol asks for a value
> that the paper does not report, the example says so and names what would be needed.

This example walks through the full similarity chain protocol for a meta-analysis
that pools mortality risk ratios across trials comparing clozapine with other
antipsychotics in schizophrenia. A risk ratio here is the risk of death on clozapine
divided by the risk of death on the comparator antipsychotic.

**Source:** Pinioti et al. (2025), systematic review and meta-analysis of the safety
of clozapine versus other antipsychotics, *The British Journal of Psychiatry*.

---

## Step 1/6: ELICIT

**Q1: What is your analytical task?**

> We want to pool mortality risk ratios across trials that compare clozapine with
> other antipsychotics in schizophrenia. The pooled estimate will support a
> treatment recommendation.

**Q2: Why integrate rather than analyze locally?**

> The treatment recommendation should rest on the trials that compared clozapine
> with other antipsychotics, not on one trial. A meta-analysis combines their effect
> estimates into one pooled summary.

*Not given in the paper:* the size of each trial and its number of deaths. A
researcher would state them here, because they set how strong the incentive to pool
is and how far the data can check the pooling.

**Q3: What do you know about both contexts?**

> - **Sources (the trials):** 11 trials that compare clozapine with other
>   antipsychotics in schizophrenia contribute mortality data. A search strategy
>   decided which trials entered the review. It defines the population,
>   interventions, comparator treatments, outcomes and study design, and it was
>   applied at title, abstract and full-text screening. The authors pre-specified
>   two moderators, male percentage and mean clozapine dose.
> - **Target (the recommendation):** Patients with schizophrenia to whom the
>   treatment recommendation will apply.

*Not given in the paper:* each trial's sample size, number of deaths, follow-up,
setting, enrollment period and comparator antipsychotic. A researcher would take
them from the supplementary material of the meta-analysis.

**Q4: What data/metadata do you have access to?**

> Trial-level results (aggregate data) as reported in the supplementary material of
> the published meta-analysis, and the protocol documents that screening relied on.

**Reflect-back:**

> **What I understood:**
> - **Analytical task:** Pool mortality risk ratios across 11 clozapine trials to
>   support a treatment recommendation
> - **Why integration is needed:** A treatment recommendation needs one summary
>   across the trials. The trial sizes and death counts that would show how strong
>   the incentive is are not yet stated.
> - **Sources:** 11 trials comparing clozapine with other antipsychotics in
>   schizophrenia, selected by a search strategy at screening
> - **Target:** Patients with schizophrenia to whom the recommendation will apply
> - **Available data:** Aggregate trial-level results and protocol documents
> - **Feasibility note:** The design condition rests on protocol documents, so it
>   can at most be assumed. The data and model conditions can be checked from the
>   trial-level results. With 11 trials, these checks are imprecise. Subgroup
>   analyses and meta-regression have low power, and τ² (the between-study variance)
>   is estimated imprecisely.

---

## Step 2/6: CLASSIFY

**Task catalog match:** 1.2 Meta-analyze

**Why this match:** The researcher combines effect estimates across studies at the
aggregate level. This is not 1.1 (Pool cohorts), because no individual-level data
are merged. It is not 1.3 (Compare treatments through a network of trials), because
that entry applies when the treatments of interest were never compared head to head.
These trials compared clozapine with other antipsychotics directly.

**Chain for this task:** The catalog entry for task 1.2 gives the chain this example
follows: design (gate), then data, then model. The data and model conditions,
evidence compatibility and synthesis adequacy, are defined in
`references/03-data_similarity.md` and `references/04-model_similarity.md`.

**Required subspaces (in assessment order):**
1. Design: operational equivalence (gate, a prerequisite that must pass before the
   later assessments are meaningful). *Do the trials define populations,
   interventions, comparators, outcomes and study design comparably?*
2. Data: evidence compatibility (graded check, whose failure calls for a
   modification rather than a stop). *Do trial attributes that the search strategy
   does not fix explain differences in the observed effects?*
3. Model: synthesis adequacy (graded check). *Is the spread of true effects narrow
   enough for the pooled summary to support the recommendation?*

---

## Step 3/6: ROADMAP

**Gated pipeline:**

```
Operational equivalence ──gate──→ Evidence compatibility ──→ Synthesis adequacy
          │                                 │                         │
      If fails:                         If fails:                 If fails:
      STOP.                             Flag: attributes          Flag: the pooled
      Effects are not                   outside the search        mean alone cannot
      comparable                        strategy explain          carry the
                                        the effects               recommendation
```

**Decision rule** (applied in Step 5):
1. Any gate not met, no remediation available → Integration not recommended
2. Any gate not met, remediation available → Integration with Remediation
3. Any gate or graded check assumed → Assumption-based Integration
4. Any graded check not met → Integration with Modifications
5. All conditions met → Unconditional Integration

The first matching rule decides, and there is exactly one recommendation.

**Why this order for this case:**
- **Design comes first** because it asks whether the trials *could* generate
  comparable data. The gap it looks for lies in the protocols, and a gap in the
  protocols cannot be closed by statistical means. If it fails, the risk ratios are
  not comparable.
- **Data comes second.** It asks whether the data the trials *did* generate are
  close on the attributes that the search strategy does not enforce. The question
  needs the design gate settled first.
- **Model comes last.** It asks whether the synthesis model supports the intended
  pooled interpretation. A random-effects mean (the average effect from a model
  that lets the true effect vary between trials) is an incomplete summary. When the
  distribution of true effects is wide, the mean alone can mislead.

**Small-data note:** 11 trials contribute mortality. At this number of trials,
subgroup analyses and meta-regression have low power, so the absence of an
association does not establish a condition. τ² is estimated imprecisely. An apparent
difference may reflect sampling variation rather than a difference between
populations, so a dissimilarity reported without an interval is difficult to act on.

**Pattern alert:** The common pattern in `references/05-failure_cases.md` applies.
The statistic that is easy to compute is I² = 0% (I² is the share of observed
variation in excess of chance). It speaks to the model subspace, while the gate lies
in the design subspace. Low observed heterogeneity is evidence about the trials that
were found. It does not demonstrate that the underlying studies are homogeneous, and
it cannot settle the design gate.

---

## Step 4/6: ASSESS

### Assessment 1/3: Design: Operational equivalence [gate]

> **This is a gate.** If the trials define their populations, interventions,
> comparators, outcomes or study designs differently, their risk ratios are not
> comparable. Downstream assessments would be meaningless.

**Condition:** The 11 trials define populations, interventions, comparator
treatments, outcomes and study design comparably, and they measure the same
constructs on the same scales.

**Evidence:** The search strategy, applied at title, abstract and full-text
screening. All 11 trials passed it. This is a documentary assessment. It rests on
protocol documents, not on measurement.

**Status: Assumed**

The documents agree, since every included trial meets the search strategy. Documents
that agree cannot show that the trials measured what the documents say, so the
condition is assumed, not met.

**Dissimilarity function:** The search strategy definition applied at screening. The
paper reports no value for it beyond the inclusion of the 11 trials. *Not given in
the paper:* the screening result per criterion (population, interventions,
comparators, outcomes, study design) for each trial. Reported as a share with its
denominator, this would be the per-criterion eligibility concordance that the
catalog lists for task 1.2.

**What would establish it:** Evidence that rests on measurement rather than on
documents, showing that each trial's population, interventions, comparators and
outcome are what its protocol says. The paper names no such evidence for this
example.

**Gate: ASSUMED.** Proceed to evidence compatibility, conditional on this assumption.

> **Progress:**
>
> | # | Subspace | Status | Role | Implication |
> |---|----------|--------|------|-------------|
> | 1 | Design: operational equivalence | Assumed ○ | Gate, assumed | Proceed, conditional on assumption |
> | 2 | Data: evidence compatibility | *up next* | Graded check | |
> | 3 | Model: synthesis adequacy | *pending* | Graded check | |

---

### Assessment 2/3: Data: Evidence compatibility [graded check]

> Taking the trials to define their populations and outcomes comparably (assumed
> above), we can ask whether the trials' observed effects differ along attributes
> that the search strategy leaves open.

**Condition:** Study and population attributes that the search strategy does not fix
do not systematically explain differences in the observed effects across the 11
trials.

**Evidence:**
- **Meta-regression** relates the trials' effect estimates to study attributes. On
  the two moderators the authors pre-specified, male percentage and mean clozapine
  dose, it showed no significant impact on the overall result.
- **Subgroup analyses** and **funnel plot asymmetry** are the other two functions
  for this condition. Funnel plot asymmetry is an association between effect size
  and precision. It indicates that study size, or an attribute linked to it, explains
  differences in effects. Publication bias (the studies found differ from the studies
  conducted) is one possible cause. *Not given in the paper:* their results for this example. A researcher
  would report the subgroup estimates with intervals and the funnel plot or an
  asymmetry test on the 11 trials.

**Status: Assumed**

The meta-regression found no association. With 11 trials it has low power, so the
absence of an association does not establish the condition. It also covers only two
attributes. Both are trial-level averages, so an association across trials need not
hold within trials (ecological bias). Study characteristics outside the scope of the search strategy remain
unmeasured. The evidence cannot establish the condition, so it is assumed. The paper
reports this evidence but does not state a status for the data link. The status
here follows from the paper's status rule.

**Dissimilarity functions:**
- Meta-regression on male percentage and mean clozapine dose: no significant impact.
  *Not given in the paper:* the coefficients with their confidence intervals.
- Subgroup analyses: *not given in the paper.*
- Funnel plot asymmetry: *not given in the paper.*

**What would establish it:** Subgroup analyses or meta-regression on the attributes
that could modify the effect, with intervals, and with enough trials that the
absence of an association becomes informative.

> **Progress:**
>
> | # | Subspace | Status | Role | Implication |
> |---|----------|--------|------|-------------|
> | 1 | Design: operational equivalence | Assumed ○ | Gate, assumed | Proceed, conditional on assumption |
> | 2 | Data: evidence compatibility | Assumed ○ | Graded check | Claim rests on assumption |
> | 3 | Model: synthesis adequacy | *up next* | Graded check | |

---

### Assessment 3/3: Model: Synthesis adequacy [graded check]

> The trials' effects show no association with the two pre-specified moderators.
> The question now is whether the spread of true effects across trials is narrow
> enough for one pooled number to support the recommendation.

**Condition:** The distribution of true effects is narrow enough for the pooled
summary to support the stated recommendation. The purpose sets the bar. A synthesis
that supports a treatment recommendation needs a narrower distribution of true
effects than one that flags a candidate signal for further study.

**Evidence:**
- Pooled mortality risk ratio 1.01 (95% CI 0.50 to 2.01) across the 11 trials.
- τ² = 0.0024. τ² is the between-study variance, the variance of the true effects
  across trials.
- I² = 0%. I² expresses the share of observed variation in excess of chance. It
  does not measure the spread of true effects.
- *Not given in the paper:* the prediction interval (the interval for the effect in
  a new trial) and an interval for τ².

**Status: Assumed**

The reported τ² is 0.0024. With 11 trials, τ² is estimated imprecisely, and the
paper reports neither its interval nor the prediction interval. Low observed
heterogeneity is evidence about the trials that were found, not a demonstration that
the underlying studies are homogeneous. The evidence cannot establish the condition,
so it is assumed. As for the data link, the paper does not state this status. It
follows from the status rule.

**Dissimilarity functions:**
- Between-study variance: τ² = 0.0024. *Not given in the paper:* its interval.
- Prediction interval: *not given in the paper.*

**What would establish it:** The prediction interval for the risk ratio in a new
trial and an interval for τ², set against the range of risk ratios over which the
treatment recommendation would stay the same.

**No concern flagged.** No graded check is not met, so no modification is required.

> **Progress (final):**
>
> | # | Subspace | Status | Role | Implication |
> |---|----------|--------|------|-------------|
> | 1 | Design: operational equivalence | Assumed ○ | Gate, assumed | Proceed, conditional on assumption |
> | 2 | Data: evidence compatibility | Assumed ○ | Graded check | Claim rests on assumption |
> | 3 | Model: synthesis adequacy | Assumed ○ | Graded check | Claim rests on assumption |

---

## Step 5/6: RECOMMENDATION

**ASSUMPTION-BASED INTEGRATION**

The design gate rests on protocol documents only. Documents that agree cannot
establish a condition, so the gate is assumed. The data and model conditions are
assumed as well. No condition is not met.

**Basis:**
- Gates: 1 assessed, 0 met, 1 assumed, 0 not met
- Graded checks: 2 assessed, 0 met, 2 assumed, 0 not met
- Deciding rule: rule 3 (a gate is assumed). Rules 1 and 2 do not match, because no
  gate is not met. The assumed gate decides the recommendation on its own. Other
  statuses for the two graded checks would not change it.

**Assumption inventory:**
1. Design: operational equivalence. The trials define populations, interventions,
   comparators, outcomes and study design comparably. Assumed because only protocol
   documents support it.
2. Data: evidence compatibility. Attributes outside the search strategy do not
   explain differences in the effects. Assumed because the meta-regression covered
   two moderators in 11 trials, and other study characteristics remain unmeasured.
3. Model: synthesis adequacy. The spread of true effects is narrow enough for the
   recommendation. Assumed because τ² from 11 trials is imprecise and no prediction
   interval is reported.

**Required modifications:** None. No graded check is not met.

**Sensitivity analysis:** Integration proceeds on assumed conditions, so quantify how
far the conclusion can move if an assumption fails. *Not given in the paper:* a
sensitivity analysis for this example.

**What this means concretely:** The pooled mortality risk ratio of 1.01 (95% CI 0.50
to 2.01) is interpretable only under the stated assumptions. Report τ² = 0.0024 next
to the design assumption, not in place of it. State the assumption inventory in the
methods section and carry it into the interpretation of the treatment recommendation.

---

## Step 6/6: REPORT

# Similarity Specification Report

## Task and context

- **Analytical task:** Pool mortality risk ratios across trials that compare
  clozapine with other antipsychotics in schizophrenia, to support a treatment
  recommendation.
- **Task catalog match:** 1.2 Meta-analyze, with the chain as the paper's Section 3
  states it (design gate, then data, then model).
- **Source context:** 11 trials contributing mortality to the meta-analysis of
  Pinioti et al. (2025), selected by a search strategy applied at title, abstract and
  full-text screening. *Not given in the paper:* each trial's population, design, n
  and enrollment period.
- **Target context:** Patients with schizophrenia to whom the treatment
  recommendation will apply. The paper does not describe this population further.
- **Reason for integration:** A treatment recommendation needs one pooled summary
  across the trials. *Not given in the paper:* the trial sizes and death counts that
  would show how strong the incentive to pool is.
- **Feasibility boundary:** The design condition rests on protocol documents and can
  at most be assumed. The data and model conditions are assessable from the trial-level
  results. With 11 trials, the functions are imprecise, and the reported evidence
  cannot establish either condition.

---

## Subspace assessments

Listed in roadmap order.

### 1. Design: Operational equivalence

- **Role:** gate
- **Condition:** The trials define populations, interventions, comparator
  treatments, outcomes and study design comparably, and they measure the same
  constructs on the same scales.
- **Status:** assumed
- **Evidence:** Documentation compared. The search strategy was applied at title,
  abstract and full-text screening, and all 11 trials passed it. Protocol documents
  only.
- **Dissimilarity function (with uncertainty):** Search strategy definition applied
  at screening, a documentary assessment without sampling uncertainty. *Not given in
  the paper:* the per-criterion result for each trial.
- **If assumed, evidence that would establish it:** Measured evidence, not documents,
  that each trial's population, interventions, comparators and outcome are what its
  protocol says.

### 2. Data: Evidence compatibility

- **Role:** graded check
- **Condition:** Study and population attributes that the search strategy does not
  fix do not systematically explain differences in observed effects.
- **Status:** assumed
- **Evidence:** Empirical test. Meta-regression on the two pre-specified moderators,
  male percentage and mean clozapine dose, showed no significant impact on the
  overall result. Study characteristics outside the search strategy remain
  unmeasured.
- **Dissimilarity function (with uncertainty):** Meta-regression, no significant
  impact. *Not given in the paper:* the coefficients and their intervals, the
  subgroup analyses, and funnel plot asymmetry.
- **If assumed, evidence that would establish it:** Subgroup analyses or
  meta-regression on the attributes that could modify the effect, with intervals,
  and with enough trials that the absence of an association becomes informative.

### 3. Model: Synthesis adequacy

- **Role:** graded check
- **Condition:** The distribution of true effects is narrow enough for the pooled
  summary to support the treatment recommendation.
- **Status:** assumed
- **Evidence:** Empirical test. Pooled mortality risk ratio 1.01 (95% CI 0.50 to
  2.01), τ² = 0.0024 and I² = 0% across 11 trials.
- **Dissimilarity function (with uncertainty):** Between-study variance τ² = 0.0024.
  *Not given in the paper:* the interval for τ² and the prediction interval.
- **If assumed, evidence that would establish it:** The prediction interval for the
  risk ratio in a new trial and an interval for τ², set against the range of risk
  ratios over which the recommendation would stay the same.

---

## Recommendation

- **Recommendation:** Assumption-based Integration
- **Basis:** Rule 3 is the first matching rule, because the design gate is assumed.
  Both graded checks are assumed as well. No condition is not met.
- **Required modifications:** None.
- **Assumption inventory:**
  - Design: operational equivalence. Assumed because only protocol documents
    support it.
  - Data: evidence compatibility. Assumed because the meta-regression covered two
    moderators in 11 trials, and other study characteristics remain unmeasured.
  - Model: synthesis adequacy. Assumed because τ² from 11 trials is imprecise and no
    prediction interval is reported.
- **Gate remediations:** None.

---

## Suggested methods-section paragraph

> Our analytical task was to pool mortality risk ratios across trials comparing
> clozapine with other antipsychotics in schizophrenia, to support a treatment
> recommendation. We applied the similarity chain protocol. We assessed operational
> equivalence (design subspace, a gate) with the search strategy applied at title,
> abstract and full-text screening. This assessment rests on protocol documents, so
> we classified the condition as assumed. We assessed evidence compatibility (data
> subspace) with meta-regression on the two pre-specified moderators, male
> percentage and mean clozapine dose. The meta-regression showed no significant
> impact on the overall result. With 11 trials it has low power, and study
> characteristics outside the search strategy remain unmeasured, so we classified
> this condition as assumed. We assessed synthesis adequacy (model subspace) with the
> between-study variance, τ² = 0.0024 (I² = 0%) [add the prediction interval]. With
> 11 trials, τ² is estimated imprecisely, so we classified this condition as assumed
> as well. The recommendation was Assumption-based Integration. The pooled mortality
> risk ratio was 1.01 (95% CI 0.50 to 2.01). It is interpretable only under the
> stated assumptions, and the reported τ² accompanies the design assumption rather
> than replacing it.

---

## Metadata

- **Date:** 2026-11-05
- **Assessed by:** AI-assisted via similarity-chain protocol (Claude Opus 5.5), from
  Section 3 of the paper
- **Data access:** mixed (protocol documents and published trial-level results)
- **Local sample size:** 11 trials contributing mortality. *Not given in the paper:*
  the number of patients and deaths.

---

## Teaching Notes

This example illustrates several key principles:

1. **Documents cannot meet a gate.** All 11 trials passed the search strategy, and
   the design gate still stays assumed. Agreeing documents cannot show that the
   trials measured what the documents say.

2. **Low heterogeneity is not homogeneity.** I² = 0% and τ² = 0.0024 describe the
   trials that were found. They do not demonstrate that the underlying studies are
   homogeneous. The reported τ² accompanies the design assumption and does not
   replace it.

3. **The purpose sets the bar for the model link.** The same meta-analysis, used to
   flag a candidate signal for further study, would tolerate a wider distribution of
   true effects than one that supports a treatment recommendation.

4. **The first matching rule decides.** The assumed design gate triggers rule 3 on
   its own. The statuses of the graded checks do not change the recommendation, but
   they enter the assumption inventory.

5. **The catalog entry is a starting point.** Task 1.2 in the catalog follows the
   chain used here. When the analytical task and its purpose call for a different
   chain, adapt the
   sequence and confirm it with the researcher (SKILL.md, Step 2).

6. **The chain orders what meta-analysis already does.** Meta-analysis defines
   eligibility before pooling, quantifies heterogeneity, and limits interpretation
   where dissimilarity cannot be explained. The chain adds an order for these
   assessments, set by what the pooled estimate is for.
