# French Motor Third-Party Liability Pricing Analysis

A notebook-based actuarial analysis of claim frequency, claim severity, and expected pure premium using the French `freMTPL2` motor third-party liability data. The project moves from data checks and descriptive exploration through development-stage model comparison to one final evaluation on a reserved policy-level test sample.

The selected frequency model is a boosted Poisson regression; the selected severity model is a Gamma GLM with a log link. On the reserved test sample, their combined expected loss was **$11.663 million** against **$11.734 million** observed (aggregate observed-to-predicted ratio **1.006**). This is portfolio-level calibration for one test realization, not a guarantee about other portfolios or individual claims. Severity remains substantially more sensitive to rare large claims than frequency.

## Project contents

- [`french_auto_pricing_report.pdf`](french_auto_pricing_report.pdf) — concise final report.
- [`notebooks/`](notebooks/) — the seven analysis notebooks, in project order:
  1. Data overview and integrity checks.
  2. Claim-frequency exploration.
  3. Claim-severity exploration.
  4. Initial frequency and severity GLMs, split definition, and diagnostics.
  5. Development cross-validation and conventional model comparison.
  6. Development-stage body/tail severity experiment.
  7. Final evaluation of the frozen specifications.
- [`images/plots/`](images/plots/) — figures saved for the report and notebook companion material.
- [`data/raw/`](data/raw/) — the two source CSV files.
- [`models/`](models/) — selected-specification JSON files and run manifests. Fitted binary model objects are regenerated and are not tracked.
- [`data/processed/`](data/processed/) — generated row-level extracts, predictions, and diagnostics. These files are not tracked; the notebooks recreate them.

## Main methodological boundary

The descriptive EDA in Notebooks 02 and 03 used the full supplied portfolio before the policy-level development/test split was made. The final test sample was excluded from model fitting, cross-validation, candidate comparison, and model selection, then evaluated once in Notebook 07. It was therefore held out from model development, but it was not unseen during the initial descriptive exploration.

The final choice was frozen before Notebook 07. The explicit body/GPD-tail experiment in Notebook 06 did not replace the Gamma severity benchmark because its estimated tail mean was unstable across folds and thresholds. Catastrophic claims were retained in the severity target throughout.

## Reproduce the analysis

Use Python 3.14 and run the notebooks in numerical order. Later notebooks use generated files from earlier stages, so running an isolated later notebook in a fresh checkout will not work until its inputs have been produced.

In PowerShell, from the project root:

```powershell
py -m venv .venv
.\.venv\Scripts\python -m pip install -r requirements.txt
```

Open the project folder in VS Code, select the `.venv` interpreter as the notebook kernel, and run the notebooks from `notebooks/` in order. The analysis outputs and binary model objects are generated locally and excluded from version control. Existing notebook outputs and the saved figures provide a readable record without requiring a full rerun.

## Data source and attribution

The policy and claim files are exports of `freMTPL2freq` and `freMTPL2sev` from the [CASdatasets R package](https://dutangc.github.io/CASdatasets/). Cite the dataset as:

> Dutang, Christophe, and Arthur Charpentier (2026). *CASdatasets: Insurance datasets*, R package version 1.2-1. DOI: [10.57745/P0KHAG](https://doi.org/10.57745/P0KHAG).

The dataset repository record lists the Etalab Open License 2.0 ([license text](https://www.data.gouv.fr/pages/legal/licences/etalab-2.0)); attribution to the source is retained here. The CASdatasets package itself is distributed under GPL (>= 2). See the linked sources for the applicable terms.

## Software

Direct Python dependencies and the versions used for this analysis are listed in [`requirements.txt`](requirements.txt). The notebooks were developed with Python 3.14.7.

## Project license

The project’s original code and documentation are licensed under the [MIT License](LICENSE). The source data remains subject to its separately documented license.
