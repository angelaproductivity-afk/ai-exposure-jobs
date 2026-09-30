# Data

The notebooks expect this folder layout. Only the final output is in the repo. Download the raw inputs from the sources below and save them under the listed paths.

```
data/
├── FINAL_mca_data.csv                         # MCA job list (01)
├── labor_codes/
│   └── 2019_to_SOC_Crosswalk.csv              # O*NET-SOC 2019 to SOC crosswalk (01)
├── ai_measurements/
│   ├── job_exposure.csv                       # Anthropic Observed Exposure, March 2026 (02)
│   ├── occ_level.csv                          # Human and LLM Beta ratings (03)
│   ├── release_2026_06_26/data/
│   │   └── aei_claude_ai_2026-06-26.csv       # Anthropic Economic Index, June 2026 release (02)
│   └── worldbank_metrics/
│       ├── input/
│       │   ├── AIOE_DataAppendix.xlsx         # Felten et al. (2021) appendix (01)
│       │   ├── abilities.csv                  # O*NET Abilities (01)
│       │   ├── work_context.csv               # O*NET Work Context (01)
│       │   └── job_zones.csv                  # O*NET Job Zones (01)
│       └── output/                            # written by 01
├── auxiliary/
│   ├── final_mca_soc_code.csv                 # MCA jobs mapped to SOC codes (02, 04)
│   ├── soc_consolidated_ai_metrics.csv        # written by 02
│   └── stage_3_archetypes.csv                 # written by 03
└── output/
    └── final_mca_with_ai_measurements_sep_28.csv   # final output (04)
```

## Sources

| File | Where to get it |
|---|---|
| O*NET Abilities, Work Context, Job Zones | O*NET Resource Center database download: https://www.onetcenter.org/database.html |
| O*NET-SOC 2019 to SOC crosswalk | O*NET Resource Center crosswalks: https://www.onetcenter.org/taxonomy.html |
| AIOE data appendix | Felten, Raj & Seamans (2021) replication data: https://github.com/AIOE-Data/AIOE |
| Observed Exposure; Human and LLM Beta | Massenkoff & McCrory (2026): https://www.anthropic.com/research/labor-market-impacts |
| Anthropic Economic Index | https://huggingface.co/datasets/Anthropic/EconomicIndex |
| MCA job list and PSOC-to-SOC crosswalk | My own crosswalk of MCA job titles (PSOC/ISCO) to US SOC codes |
