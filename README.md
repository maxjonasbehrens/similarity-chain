# The Similarity Chain

The similarity chain is a protocol for data integration. It specifies which similarity between data sources an analytical task requires. The chain has four links: the analytical task, the subspaces it selects (design, data or model), a condition for each subspace, and a dissimilarity function that assesses the condition. Some subspaces act as gates. If a gate's condition is not met, the later comparisons are ill-defined, so gates are assessed first. The protocol ends in one of five recommendations: Unconditional Integration, Integration with Modifications, Assumption-based Integration, Integration with Remediation or Integration not recommended.

This repository holds the resource described in the preprint:

> Behrens M, Kober N, Farhadizadeh M, Habenicht H, Hackenberg M, Kabus F, Kwarteng N, Pfaffenlehner M, Schächter C, Binder H, Binder N, Hertel J. The Similarity Chain: A Specification Protocol for Data Integration. arXiv, 2026.

## Contents

| Path | Content |
|---|---|
| `similarity-chain/references/01-decision_catalogue.md` | Task catalog: 20 integration tasks, each with the gates and graded checks it requires |
| `similarity-chain/references/02-design_similarity.md` | Conditions and dissimilarity functions for the design subspace |
| `similarity-chain/references/03-data_similarity.md` | Conditions and dissimilarity functions for the data subspace |
| `similarity-chain/references/04-model_similarity.md` | Conditions and dissimilarity functions for the model subspace |
| `similarity-chain/references/05-failure_cases.md` | Three published failures where the similarity checked was not the one the task required |
| `similarity-chain/references/06-report_template.md` | Template for the similarity specification report |
| `similarity-chain/examples/` | Worked example: transport of the Framingham risk score (an extension task outside the combined-data scope of the preprint) |
| `similarity-chain/SKILL.md` | Agent skill that guides an analyst through the chain in conversation |
| `similarity-chain.zip` | The skill folder, packed for upload |

## Using the agent skill

- Claude Code: copy the folder `similarity-chain/` into `~/.claude/skills/` (all projects) or into `.claude/skills/` of one project.
- Claude apps: upload `similarity-chain.zip` as a custom skill in the settings.
- Other agents that read `SKILL.md` skills can use the folder in the same way.

The skill has not yet been tested with users. The user study in the preprint tested a plain-language specification drawn from the same task catalog.

## Status

This is the version for the arXiv preprint. The code for the plasmode simulations and the user study will be added before journal publication, together with an archived release on Zenodo.

## License

The catalogs, the agent skill and this documentation are licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Code added later will be licensed under the MIT license.
