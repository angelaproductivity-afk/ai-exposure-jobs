# Concept Note: AI Exposure and Usage for MCA Job Roles

Angela Chen-Delantar · September 2026

## Purpose

I am attaching AI-exposure measures to the job titles in My Career Advisor (MCA), a career guidance tool for Philippine students. The goal is to tell learners and schools, for each MCA career, how AI is likely to change the work and what that means for preparation.

The analysis follows a funnel of three questions:

1. **What could AI technically affect?** Theoretical exposure, from the AIOE index (Felten et al., 2021) and its complementarity-adjusted version, C-AIOE.
2. **Where is generative AI actually being used?** Observed usage, from Anthropic's Observed Exposure measure.
3. **When AI is used, what role does it play?** Automation versus augmentation, from the Anthropic Economic Index.

At each step I test one link: does exposure turn into use, and does use mean automation?

**Repository:** code, outputs and data notes are in this repository (see the [README](../README.md)). This note asks for your view on two open design choices, set out in Questions 1 and 2 below.

## What is built

Stages 1 to 3 are built at the SOC occupation level, and the MCA link (Stage 4) is built as a crosswalk. Of 1,016 SOC occupations, 537 have every measure needed for Stages 1 to 3. Of 1,151 MCA jobs, 852 (74%) receive an archetype, across 395 SOC codes.

| Stage | What it does | Status |
| --- | --- | --- |
| 1. Exposure vs. usage | Builds AIOE, complementarity and C-AIOE for every SOC code, joins Observed Exposure, and compares them (correlation 0.52 between C-AIOE and Observed Exposure) | Built. The Exposure-Usage Gap score is not yet built |
| 2. Transition archetypes | Sorts each occupation into a 2x2 of theoretical exposure (high/low) against observed usage (high/low), then tests stability with Cohen's Kappa | Built and fixed; rerun pending. Measure and threshold are Questions 1 and 2 |
| 3. Automation vs. augmentation | Uses Anthropic's interaction shares: Directive and Feedback Loop count as automation; Learning, Task Iteration and Validation count as augmentation. Active Transformation jobs split at a 50% augmentation share into AI-Powered or Transition Pressure | Built and fixed; rerun pending |
| 4. Link to MCA | MCA job → PSOC → ISCO → SOC → AI measures and archetype | Crosswalk built. Learner implications not yet written |

The four archetypes, and what each could mean for learners:

| Archetype | Theoretical exposure | Observed usage | Possible implication for learners |
| --- | --- | --- | --- |
| Active Transformation | High | High | AI is already changing this work. If use is mostly augmentation (AI-Powered), learning to work with AI is a career advantage now. If mostly automation (Transition Pressure), curriculum and entry-level preparation may need bigger changes |
| Latent Transformation | High | Low | Change has not arrived yet, but learners and schools should prepare for it |
| AI-Enabled Expansion | Low | High | Use runs ahead of what capability indices predict; may reveal new applications |
| Limited Current Transformation | Low | Low | Little change for now; useful for learners to know too |

## What changed since the first draft

A code review found one bug that changed the results. It is now fixed, and I am rerunning Stages 2 and 3. Until the rerun is done, archetype counts are provisional.

| Issue | Effect | Fix |
| --- | --- | --- |
| The threshold step reused a leftover loop variable | The archetypes labelled "C-AIOE" were actually built from Human Beta. For example, Bookkeeping Clerks (C-AIOE in the 93rd percentile) were classed as low theoretical exposure | The step now names C-AIOE explicitly |
| The AI-Powered / Transition Pressure split was applied to every job | 76 Limited Current jobs were tagged "Transition Pressure", against the plan in this note | The split now applies to Active Transformation only |
| Notebook text had drifted from the outputs | The correlations quoted were 0.59 and 0.64. The actual values are 0.54 (Human Beta) and 0.61 (LLM Beta). The sample size quoted was 756 SOC codes; the actual is 537 | Text updated to match the outputs |
| Release date was dropped from the final table | Results could not be traced to an Anthropic data release | The release date is now carried through to the output |

One upside: the accidental run already shows what the archetypes look like under Human Beta. That makes Question 1 below a real comparison rather than a hypothetical one.

## Question 1: Which measure should define theoretical exposure?

My current leaning is C-AIOE as the main measure, with Human Beta as a robustness check. The three candidates agree only partly, so the choice changes which archetype some jobs fall into.

| Measure | What it captures | Correlation with Observed Exposure | For | Against |
| --- | --- | --- | --- | --- |
| AIOE (Felten et al., 2021) | How much a job's abilities overlap with AI capabilities | Not tested as a main measure | Simple and widely cited | Exposure only. It cannot tell whether AI would replace or support the worker |
| C-AIOE (Pizzinelli et al., 2023 adjustment) | AIOE, reduced for jobs where the work context shields humans (responsibility, face-to-face work, high-stakes decisions) | 0.52 | Built independently of Anthropic's data, so the comparison with observed use is a real test. Accounts for complementarity, as Observed Exposure accounts for automation | Complementarity weights are a modelling choice. Built for cross-country comparison, not for generative AI specifically |
| Human Beta (Eloundou et al., 2023) | Human-rated share of a job's tasks an LLM could speed up | 0.54 | The benchmark Anthropic uses. Specific to LLMs. Human raters rather than a model | An input to Observed Exposure itself, so comparing the two is partly circular |

**How much the choice matters.** At the same threshold, archetypes from C-AIOE and Human Beta agree at a Cohen's Kappa of 0.61, which counts as substantial on Landis and Koch's (1977) scale. That still leaves a meaningful share of jobs in a different group. The fixed notebook now reports that share and a cross-table of which jobs move. The two beta versions agree more closely with each other (Kappa 0.80), which is expected because they rate the same tasks.

**Why complementarity matters here.** Moving from AIOE to C-AIOE reorders many individual jobs even though the average does not change: the standard deviation of the rank change is 0.25 on a 0 to 1 scale. Within Legal, judges fall 0.72 and lawyers fall 0.46, while paralegals rise 0.12.

**What I would like your view on:**

- Is independence from the observed measure (C-AIOE) or comparability with Anthropic's own benchmark (Human Beta) more defensible for this purpose?
- If C-AIOE is the main measure, is reporting Human Beta archetypes alongside it enough of a robustness check?
- Should the LLM-rated beta (Kappa 0.80 with Human Beta, correlation 0.61) be dropped, given it adds little beyond Human Beta?

## Question 2: Where should "high" and "low" be drawn?

The current rule is not a plain 50th percentile, and the two axes use different rules. I would like to agree the rule before finalising the archetypes.

**How the current rule works:**

- **Theoretical axis:** high if the job is above the median C-AIOE percentile. This splits jobs 50/50.
- **Usage axis:** Observed Exposure is heavily skewed. At least a quarter of the 537 occupations have zero usage, the median is 0.03 and the mean is 0.10. So I treat zero as "low" automatically, then split the remaining jobs at *their* 50th percentile (about 0.12 in the previous run).
- **Result:** "high usage" means roughly the top quarter to third of all jobs, not the top half. So far I have only tested cut-offs from the 50th to 59th percentile of non-zero jobs.

**Options the fixed notebook now compares:**

| Usage rule | Logic | Trade-off |
| --- | --- | --- |
| Non-zero jobs, 50th percentile (current) | Separates "some use" from "substantial use" | Different from the theory axis; harder to explain |
| Non-zero jobs, 25th or 75th percentile | Looser or stricter bar for "high" | Tests how sensitive the groups are |
| Median of all jobs | Same rule as the theory axis | With so many zeros, the median sits very low, so almost any use counts as "high" |
| Above the mean (z > 0) | Standardised score, as the original concept note suggested | Sensitive to a few very high-usage jobs |

For each rule the notebook reports the group sizes, the share of jobs rated high usage, the share that switch group compared with the current rule, and Cohen's Kappa against it.

**How I propose to choose:**

1. **Stability:** small changes to the cut-off should not move many jobs (Kappa above 0.6 against neighbouring rules).
2. **Usable group sizes:** AI-Enabled Expansion had only 19 jobs in the previous run, which is too few to interpret.
3. **Fit with the framework:** Latent Transformation should be larger than Active Transformation, since use lags capability.
4. **Explainable to learners:** the rule should be simple enough to state in one sentence on the MCA site.

**What I would like your view on:**

- Is it defensible to treat zero usage separately and use different rules on the two axes, or should both axes use the same rule?
- Should the threshold be picked for stability (the data decides), for theory fit (the framework decides), or both?
- Would a middle band ("moderate") be more honest than a hard high/low split for jobs near the cut-off?

## Limitations and next steps

**Limitations:**

- **US usage, Philippine jobs.** Observed usage comes from Claude conversations, mostly outside the Philippines. How the same job is done in the Philippines may differ.
- **One AI product.** Claude usage may not reflect use of other AI tools, and it over-represents people who already use AI.
- **Coverage gaps.** 26% of MCA jobs have no archetype, mostly in manufacturing, construction, health and agriculture.
- **One snapshot.** The analysis uses a single Anthropic release (June 2026, data to May 2026), not a trend over time.
- **Archetypes are conversation starters.** They describe groups of tasks, not predictions about any one person's job.

**Next steps:**

- [ ] Rerun Stages 2 and 3 with the fixed code, and update all counts
- [ ] Decide the theoretical measure (Question 1)
- [ ] Decide the usage threshold (Question 2)
- [ ] Build the Exposure-Usage Gap score from Stage 1
- [ ] Write learner implications for each MCA career (Stage 4)
- [ ] Revisit earlier Anthropic releases for a view over time, once the archetypes are settled

## Key references

| Source | Used for | Link |
| --- | --- | --- |
| Felten, Raj & Seamans (2021). Occupational, industry, and geographic exposure to artificial intelligence. *Strategic Management Journal* | AIOE index (Stage 1) | [DOI](https://doi.org/10.1002/smj.3286) · [data](https://github.com/AIOE-Data/AIOE) |
| Pizzinelli, Panton, Mendes Tavares, Cazzaniga & Li (2023). Labor market exposure to AI: Cross-country differences and distributional implications. IMF Working Paper 23/216 | Complementarity index and C-AIOE (Stage 1) | [IMF](https://www.imf.org/en/Publications/WP/Issues/2023/10/04/Labor-Market-Exposure-to-AI-Cross-country-Differences-and-Distributional-Implications-539656) |
| Eloundou, Manning, Mishkin & Rock (2023). GPTs are GPTs: An early look at the labor market impact potential of large language models | Human and LLM Beta (Question 1) | [arXiv](https://arxiv.org/abs/2303.10130) · [data](https://github.com/openai/GPTs-are-GPTs) |
| Massenkoff & McCrory (2026). Labor market impacts of AI: A new measure and early evidence. Anthropic | Observed Exposure (Stage 1) | [Anthropic](https://www.anthropic.com/research/labor-market-impacts) |
| Handa et al. (2025). Which economic tasks are performed with AI? Evidence from millions of Claude conversations | Automation and augmentation definitions (Stage 3) | [arXiv](https://arxiv.org/abs/2503.04761) |
| Anthropic Economic Index, release of 26 June 2026 (data to 1 May 2026) | Interaction shares by occupation (Stage 3) | [Anthropic](https://www.anthropic.com/economic-index) · [data](https://huggingface.co/datasets/Anthropic/EconomicIndex) |
| O\*NET database: Abilities, Work Context, Job Zones | Inputs to AIOE and complementarity | [O\*NET](https://www.onetcenter.org/database.html) |
| Philippine Standard Occupational Classification (PSOC) | MCA crosswalk (Stage 4) | [PSA](https://psa.gov.ph/classification/psoc) |
| Cohen (1960). A coefficient of agreement for nominal scales. *Educational and Psychological Measurement* | Cohen's Kappa (Questions 1 and 2) | [DOI](https://doi.org/10.1177/001316446002000104) |
| Landis & Koch (1977). The measurement of observer agreement for categorical data. *Biometrics* | Kappa interpretation bands | [DOI](https://doi.org/10.2307/2529310) |
