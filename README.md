# womens-health-microbiome-reproducibility

Are published microbiome signatures for PCOS and endometriosis reproducible across public cohorts, analysis choices and metadata quality?

A reproducibility and data-audit study using public sequencing data, Snakemake, R and Python.

---

## Motivation

Women's health conditions such as PCOS (polycystic ovary syndrome, often called PCOD in everyday use in India) and endometriosis are common and under-researched. Microbiome studies report associations with both, but published summaries do not always agree. For example, meta-analyses of the PCOS gut microbiome differ on whether alpha diversity is reduced. Public datasets also often lack the metadata that matters most for these conditions (age, BMI, hormonal contraception, menstrual cycle phase, testosterone level, disease stage).

This project asks how much of the reported signal survives consistent reprocessing, cohort-level validation and different analysis choices, and how usable the public data really are.

## Research questions

1. **PCOS gut microbiome:** After reprocessing raw public data with one consistent pipeline, is gut microbial composition different in PCOS vs controls? Is alpha diversity different (reviews disagree)?
2. **Generalization:** Does a classifier trained on some PCOS cohorts work on a held-out cohort (leave-one-cohort-out)?
3. **Endometriosis (gut vs vaginal):** Can the reported pattern of altered fecal but not vaginal composition be reproduced from the public 16S data?
4. **Robustness:** How much do conclusions change with normalization, differential abundance method and filtering?
5. **Metadata audit:** What fraction of public PCOS / endometriosis microbiome datasets carry the metadata needed to control for confounders (age, BMI, hormonal treatment, cycle phase, testosterone, disease stage)?

## Approach (planned)

| Step | Plan |
|---|---|
| Data | Public raw 16S / shotgun data from SRA/BioProject. Accessions, download dates and metadata completeness recorded in `docs/datasets.md`. |
| Search terms | Use both "PCOS" and "PCOD" plus "polycystic ovary", "endometriosis", "vaginal microbiome", "menstrual cycle" when finding datasets |
| Processing | One consistent pipeline (DADA2 for 16S, or MetaPhlAn/Kraken2 for shotgun) in Snakemake with Conda environments |
| Differential abundance | MaAsLin2 and/or ANCOM-BC2, with cohort as covariate |
| Prediction | CLR transform, regularized logistic regression / random forest, cross-validation and leave-one-cohort-out |
| Robustness grid | Normalization x DA method x filtering threshold; all results saved |
| Metadata audit | Table of datasets by availability of each key variable |
| Confounders | Where metadata allow: age, BMI, hormonal contraception, cycle phase, testosterone subgroup |

The analysis plan is written and committed **before** looking at outcomes. Later changes are dated amendments (`docs/analysis-plan.md`).

## Repository structure

```
.
├── README.md
├── LICENSE
├── config/
├── envs/
├── workflow/
│   ├── Snakefile
│   ├── rules/
│   └── scripts/
├── notebooks/
├── results/
└── docs/
    ├── analysis-plan.md
    ├── datasets.md
    ├── metadata-audit.md
    ├── references.md
    └── log.md
```

## Progress

- [ ] Literature review and `docs/references.md`
- [ ] Dataset search and metadata audit
- [ ] Analysis plan committed
- [ ] Reprocessing of PCOS gut cohorts
- [ ] Leave-one-cohort-out validation
- [ ] Endometriosis gut vs vaginal reproduction
- [ ] Robustness grid
- [ ] Report and release (Zenodo DOI)

## Expected limitations

- Few public endometriosis datasets exist, so cross-cohort validation there will be limited.
- Public metadata are often incomplete, which limits confounder control and subgroup analysis.
- Cohorts differ in region, diet, sequencing region (e.g. V3-V4 vs V4), extraction kit and diagnosis criteria (PCOS criteria vary).
- Results are associations, not causal claims, and are not clinical advice.

## Starting references (verify details on PubMed before citing)

- Torraco et al., 2025, *Medicina* 61:888: 16S meta-analysis of fecal and vaginal microbiota in endometriosis (doi:10.3390/medicina61050888)
- Min et al., 2023, *Frontiers in Microbiology*: gut microbiome and PCOS, meta-analysis plus Mendelian randomization (doi:10.3389/fmicb.2023.1203902)
- Yang et al.: individual-level analysis of public gut microbiota data in PCOS (look up the full citation on PubMed)
- Wirbel et al., 2019, *Nature Medicine*; Nearing et al., 2022, *Nature Communications*; Gloor et al., 2017, *Frontiers in Microbiology* (methods background)

## Data and ethics

Only public, de-identified datasets are used. No proprietary or employer data are included in this repository.

## License

MIT. See [LICENSE](LICENSE).

## Author

Ananya KR, bioinformatician working in microbiome analysis. · [GitHub](https://github.com/ananyakr24)
