# AI Exposure for Philippine Careers (MCA)

This is an exploratory analysis to attach AI-exposure measures to the job titles in My Career Advisor (MCA), a career guidance tool for Philippine students. It compares **theoretical** AI exposure (what AI could do in a job) with **observed** AI usage (what people actually use AI for). Then it sorts each occupation into an AI transition archetype. The MCA application is one use case and the framework could extend to other uses as well.

## Status (September 30 2026)

- **Fixed:** the archetype step now uses C-AIOE. The previous run used Human Beta. 
- **Fixed:** the AI-Powered / Transition Pressure split now applies to Active Transformation only.
- **Added:** comparison cells for the two open questions (which theoretical measure to use, and which usage threshold).
- **Pending:** rerun notebooks 03 and 04. Until then, the printed outputs from Step 2c onward and the CSV in `data/output/` reflect the previous run.

## What's in this repo

- **[`docs/concept_note.md`](docs/concept_note.md):** the research plan, what has changed, and the two open design questions.


| Notebook | What it does |
|---|---|
| `01_calculating_worldbank.ipynb` | Calculates AIOE (Felten et al., 2021) and complementarity (Pizzinelli et al., 2023), then combines them into a complementarity-adjusted measure, C-AIOE |
| `02_calculating_anthropic.ipynb` | Joins Observed Exposure (March 2026) and Anthropic Economic Index automation/augmentation data (June 2026 release) to SOC codes, and checks coverage for MCA jobs |
| `03_comparison_theoretical_observed_ai_exposure.ipynb` | Compares C-AIOE with Observed Exposure, defines the four archetypes, tests their stability with Cohen's Kappa, and looks at automation vs. augmentation |
| `04_combining_mca_ai_exposure.ipynb` | Merges the archetypes and AI measures onto the MCA job list |

**Output:** `data/output/final_mca_with_ai_measurements_sep_28.csv` has 1,151 MCA jobs with their PSOC, ISCO and SOC codes, AI measures, archetype and sub-archetype.

## The four archetypes

| | Low observed usage | High observed usage |
|---|---|---|
| **High theoretical exposure** | Latent Transformation | Active Transformation |
| **Low theoretical exposure** | Limited Current Transformation | AI-Enabled Expansion |

Active Transformation jobs are further split by augmentation share: **AI-Powered** (above 50%) or **Transition Pressure** (50% or below).

## Running the notebooks

- Run them in order, 01 to 04, from inside `notebooks/`.
- The notebooks read input files from `../data/`. I have not included the raw source files. See [`data/README.md`](data/README.md) for the folder layout and where to download each one.
- Install packages with `pip install -r requirements.txt`.

## Limitations

- The observed usage data comes from US Claude usage. It may not reflect how work is done in the Philippines.
- About a quarter of MCA jobs have no Anthropic data. Most of these are in manufacturing, construction and health.
- The AI-Enabled Expansion group is small and sensitive to where the threshold falls.
- The archetypes are a starting point for career conversations. They are not predictions about any single job.

## Sources

- Felten, E., Raj, M., & Seamans, R. (2021). Occupational, industry, and geographic exposure to artificial intelligence. *Strategic Management Journal*. https://doi.org/10.1002/smj.3286
- Pizzinelli, C., Panton, A., Mendes Tavares, M., Cazzaniga, M., & Li, L. (2023). *Labor market exposure to AI: Cross-country differences and distributional implications*. IMF Working Paper.
- Eloundou, T., Manning, S., Mishkin, P., & Rock, D. (2023). GPTs are GPTs. https://arxiv.org/abs/2303.10130
- Massenkoff, M., & McCrory, P. (2026). *Labor market impacts of AI: A new measure and early evidence*. Anthropic. https://www.anthropic.com/research/labor-market-impacts
- Anthropic Economic Index. https://www.anthropic.com/economic-index
