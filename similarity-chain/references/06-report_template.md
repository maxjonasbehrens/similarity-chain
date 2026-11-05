# Similarity Specification Report Template

Fill in the sections below based on the completed assessment. Replace all bracketed
placeholders with specific content. The structure mirrors the report template in the
supplement ("Similarity specification report template").

---

# Similarity Specification Report

## Task and context

- **Analytical task:** [State the task precisely, e.g., "Pool the target cohort (n = 120)
  with fourteen external studies to fit a 6-month mortality model for the target setting."]
- **Task catalog match:** [Reference the catalog entry, e.g., "1.1 Pool cohorts"]
- **Source context:** [Brief description of each source: name or label, population, design, n, enrollment period]
- **Target context:** [Brief description of the target: population, design, n, what the result is for]
- **Reason for integration:** [Why local data alone are insufficient, or the other motivation for integrating]
- **Feasibility boundary:** [Given available data and sample sizes, which conditions the
  evidence could settle and which had to remain assumptions]

---

## Subspace assessments

Listed in roadmap order.

### 1. [Subspace]: [Condition]

- **Role:** gate / graded check
- **Condition:** [in plain language, tailored to these contexts]
- **Status:** met / assumed / not met
- **Evidence:** [documentation compared, data structure inspected, empirical test, domain knowledge, or nothing]
- **Dissimilarity function (with uncertainty):** [name, value, interval where applicable]
- **If assumed, evidence that would establish it:** [specific data or check]
- **If gate not met, remediation:** [the structural remedy, e.g., restriction to a subpopulation or harmonized outcome definitions] / none (Integration not recommended)
- **If the dissimilarity function belongs to another subspace:** subspace measured [~], subspace claimed [~], bridging assumption [~]

### 2. [Subspace]: [Condition]

[Repeat for all required subspaces.]

---

## Recommendation

- **Recommendation:** Unconditional Integration / Assumption-based Integration / Integration with Modifications / Integration with Remediation / Integration not recommended
- **Basis:** [the first matching rule and the statuses that triggered it]
- **Required modifications (if any):** [which graded check, what to do]
- **Assumption inventory (if any):** [each assumed condition and the reason it is assumed]
- **Gate remediations (if any):** [which gate, what failed, the remediation, what is now conditional on it]

---

## Suggested methods-section paragraph

> [Adapt: "Our analytical task was to .... [Reason for integration.] We therefore
> [pooled / transported / borrowed] from [source]. We assessed [subspace] similarity with
> [dissimilarity function] and classified the condition as [status], .... The
> recommendation was [recommendation]. [Assumptions / modifications as applicable.]"]

---

## Metadata

- **Date:** [YYYY-MM-DD]
- **Assessed by:** [researcher / "AI-assisted via similarity-chain protocol", model and version]
- **Data access:** raw data / summary statistics / protocols only / mixed
- **Local sample size:** [n, events]
