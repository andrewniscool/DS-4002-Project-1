# DS-4002-Project-1

This repository contains Project 1 for Group 4 in DS 4002.

This project examines whether sentiment in selected Marvel movie reviews is associated with how long after release the reviews were posted. Using the supplied dataset, the overall Spearman correlation was close to zero and not statistically significant (ρ = −0.02, p = 0.60).

## Changes from MI2

MI2 originally proposed regression controls for movie or franchise and a comparison with TMDB ratings. As the project developed, those pieces were not included in the final MI3 analysis. The current results use Spearman correlations, timing-window comparisons, unadjusted regression models, franchise and per-movie plots, and a movie-level ANOVA, so they should be read as unadjusted associations rather than controlled effects.

## Section 1: Software and platform section 

We used Visual Studio (VS) Code and RStudio. For VS Code, we installed packages such as pandas, requests, nltk, matplotlib, numpy, and seaborn. For RStudio, we installed packages such as ggplot2, car, dplyr, and knitr. We used Mac as our platform for everything.

## Section 2: A Map of your documentation. 
```text
DS-4002-Project-1/
│
├── DATA/
│   ├── marvel_movie_reviews.csv
│   ├── marvel_movie_reviews_lexicon_sentiment.csv
│   └── README.md
│
├── OUTPUT/
│   ├── 1_scatter_lowess.png
│   ├── 2_box_windows.png
│   ├── 3_by_franchise.png
│   ├── 4_per_movie_rho.png
│   ├── 5_timing_hist.png
│   ├── 6_pos_neg_rate.png
│   ├── 7_Scatterplot_polarity_vs_days.png
│   ├── 8_Linear_Regression_Results.png
│   ├── 9_Linear_Regression_Assumptions.png
│   ├── 10_Exponential_Regression_Results.png
│   ├── 11_Quadratic_Regression_Results.png
│   ├── 12_Model_Comparison_table.png
│   └── 13_ANOVA_Results.png
│
├── SCRIPTS/
│   ├── sentiment_time_analysis.ipynb
│   ├── TMDB_Lexicon_Sentiment.ipynb
│   ├── TMDB_Script.ipynb
│   └── Statistical Analysis and Hypothesis Testing .Rmd
│
├── LICENSE.md
├── requirements.txt
└── README.md
```

## Section 3: Instructions for reproducing your results.  

These steps rebuild the dataset from the TMDB API, compute the sentiment scores, reproduce the Python figures, and rerun the R statistical analysis. Run the steps in order; each step produces the input for the next.

### Step 0: Clone the repository and install Python dependencies

1. Install **Python 3.10 or later** and Git.
2. Clone this repository and move into its root directory:

```bash
git clone https://github.com/andrewniscool/DS-4002-Project-1.git
cd DS-4002-Project-1
```

3. Create and activate a virtual environment.

On macOS or Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

On Windows PowerShell:

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

4. Install every required Python package from the repository's dependency file:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

5. Install a current version of **R** from https://cran.r-project.org/. RStudio is not required. From the VS Code terminal, install the required R packages once:

```bash
Rscript -e "install.packages(c('dplyr', 'ggplot2', 'car', 'knitr'), repos='https://cloud.r-project.org')"
```

To reproduce the exact checked-in analysis, you may skip Steps 1 and 2 below and begin with Step 3 using the supplied `DATA/marvel_movie_reviews.csv`. TMDB reviews can change over time, so recollecting them may produce a different dataset and different results.

### Step 1: Get a TMDB API key

1. Create a free account at https://www.themoviedb.org/signup.
2. Go to **Settings → API** and request an API key (choose "Developer," non-commercial use).
3. Copy your TMDB **API Key (v3 auth)**. The collection notebook passes this value through TMDB's `api_key` parameter.
4. Set the key as an environment variable before executing the collection notebook, or if using VS code, paste it when prompted to while running the TMDB_Script.ipynb notebook.

On macOS or Linux:

```bash
export TMDB_API_KEY="your_key_here"
```

On Windows PowerShell:

```powershell
$env:TMDB_API_KEY = "your_key_here"
```

When the notebook is run interactively without this environment variable, it securely prompts for the key instead of storing it in the repository.

### Step 2: Collect the reviews

1. From the repository root, execute the collection notebook:

```bash
python -m jupyter nbconvert --to notebook --execute --inplace --ExecutePreprocessor.timeout=600 "SCRIPTS/TMDB_Script.ipynb"
```
2. The notebook queries the TMDB API for English-language user reviews of the 21 films in the study and saves the raw reviews to `DATA/marvel_movie_reviews.csv`.
3. Each row contains `movie_id`, `movie`, `release_date`, `franchise`, `review_id`, `review`, `review_date`, and `days_since_release`.

### Step 3: Preprocess and compute sentiment

1. Execute the sentiment-scoring notebook from the repository root:

```bash
python -m jupyter nbconvert --to notebook --execute --inplace --ExecutePreprocessor.timeout=600 "SCRIPTS/TMDB_Lexicon_Sentiment.ipynb"
```
2. This step:
   - Loads the raw reviews from `DATA/marvel_movie_reviews.csv`.
   - Tokenizes alphabetic words and contractions and converts the tokens to lowercase.
   - Downloads NLTK's Bing Liu Opinion Lexicon when it is not already installed.
   - Counts token matches against the lexicon's positive and negative word lists.
   - Computes the polarity score for each review:
     **polarity = (positive word count − negative word count) / total word count**
3. The output is saved as `DATA/marvel_movie_reviews_lexicon_sentiment.csv`, with columns including `movie`, `franchise`, `days_since_release`, `positive_word_count`, `negative_word_count`, `total_word_count`, and `polarity_score`.

### Step 4: Run the analysis and generate the figures

1. Execute the analysis notebook from the repository root:

```bash
python -m jupyter nbconvert --to notebook --execute --inplace --ExecutePreprocessor.timeout=600 "SCRIPTS/sentiment_time_analysis.ipynb"
```

   Alternatively, open the notebook in JupyterLab or VS Code and select **Restart Kernel and Run All Cells**.
2. The notebook prints the summary statistics and saves six figures directly to the `OUTPUT/` folder:

   | File | What it shows |
   |------|---------------|
   | `1_scatter_lowess.png` | Polarity vs. years since release, with a LOWESS trend line |
   | `2_box_windows.png` | Polarity by timing window (≤1 month, 1 month–1 year, 1–5 years, 5+ years) |
   | `3_by_franchise.png` | Polarity vs. time, one panel per franchise |
   | `4_per_movie_rho.png` | Spearman ρ for each film with ≥8 reviews |
   | `5_timing_hist.png` | Distribution of when reviews were posted |
   | `6_pos_neg_rate.png` | Positive vs. negative word rate by timing window |
3. Confirm that the six figure files appear in the `OUTPUT/` folder.

### Step 5: Run the R statistical analysis

The R Markdown file contains the statistical analysis used for outputs 7-13. To review the results interactively, open `SCRIPTS/Statistical Analysis and Hypothesis Testing .Rmd` in RStudio or VS Code and run its R chunks in order.

To reproduce the R results from the repository root, run:

```bash
Rscript -e 'setwd("SCRIPTS"); knitr::opts_chunk$set(fig.path="../OUTPUT/r-analysis-figures/figure-"); knitr::knit("Statistical Analysis and Hypothesis Testing .Rmd", output="../OUTPUT/statistical_analysis.md")'
```

This creates `OUTPUT/statistical_analysis.md`, containing the numerical results, and `OUTPUT/r-analysis-figures/`, containing plots generated directly from the unchanged R Markdown file. The numbered PNG files below were exported during the original RStudio analysis and remain included as the reference presentation outputs; the command does not overwrite them:

| File | What it shows |
|------|---------------|
| `7_Scatterplot_polarity_vs_days.png` | Polarity versus days since release |
| `8_Linear_Regression_Results.png` | Linear-regression results |
| `9_Linear_Regression_Assumptions.png` | Linear-regression diagnostic plots |
| `10_Exponential_Regression_Results.png` | Exponential-regression fit |
| `11_Quadratic_Regression_Results.png` | Quadratic-regression fit |
| `12_Model_Comparison_table.png` | Regression-model comparison |
| `13_ANOVA_Results.png` | One-way ANOVA results by movie |

### Step 6: Check your results!

If you used our provided dataset, the notebook output should match these values:

| Result | Expected value |
|--------|----------------|
| Number of reviews | 486 (21 films, 4 franchises) |
| Overall Spearman correlation (days since release vs. polarity) | ρ = −0.02, p = 0.60 |
| Spearman correlation, reviews with ≥10 words only | ρ = 0.01, p = 0.77 (n = 467) |
| Kruskal–Wallis test across timing windows | H = 14.0, p = 0.003 |
| Median polarity by window (≤1 mo / 1 mo–1 yr / 1–5 yrs / 5+ yrs) | 0.017 / 0.030 / 0.055 / 0.033 |
| Avengers within-franchise Spearman correlation | ρ = 0.23, p = 0.003 |
| Reviews with polarity exactly 0 | 58 |
| Reviews posted before release (negative days) | 2 |

All tests are two-sided with α = 0.05. Small differences (in the third decimal place) can come from package versions; larger differences usually mean the data were re-collected (see the note in Step 2).

However, movie reviews can always change. People can delete them, add new reviews, or edit them at any time. If you rerun this project and collect new, more recent data, your results could differ from ours.