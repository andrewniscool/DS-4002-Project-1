# DS-4002-Project-1

This repository contains Project 1 for Group 4 in DS 4002.
## Section 1: Software and platform section 

We used Visual Studio (VS) Code and RStudio. For VS Code, we installed packages such as pandas, requests, nltk, matplotlib, numpy, and seaborn. For RStudio, we installed packages such as ggplot2, car, and dplyr. We used Mac as our platform for everything.

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
│   └── 4_per_movie_rho.png
│   └── 5_timing_hist.png
│   └── 6_pos_neg_rate.png
│   └── [figure_name].png
│
├── SCRIPTS/
│   ├── sentiment_time_analysis.ipynb
│   ├── TMDB_Lexicon_Sentiment.ipynb
│   └── TMDB_Script.ipynb
│
├── LICENSE
└── README.md
```

## Section 3: Instructions for reproducing your results.  

These steps rebuild the dataset from the TMDB API, compute the sentiment scores, and reproduce every figure and statistic in our Results. Run the steps in order; each step produces the input for the next.

### Step 0: Requirements

1. Install **Python 3.10 or later** and **Jupyter** (JupyterLab, Jupyter Notebook, VS Code, or Google Colab all work).
2. Clone this repository and move into it:
```bash
   git clone <repo-url>
   cd <repo-name>
```
3. Install the required packages:
```bash
   pip install pandas numpy matplotlib seaborn scipy statsmodels requests
```

### Step 1: Get a TMDB API key

1. Create a free account at https://www.themoviedb.org/signup.
2. Go to **Settings → API** and request an API key (choose "Developer," non-commercial use).
3. Copy your **API Read Access Token** or **API key**.
4. Paste it into `<SCRIPTS/collect_reviews.py>` where indicated (`API_KEY = "..."`), or set it as an environment variable:
```bash
   export TMDB_API_KEY="your_key_here"
```

### Step 2: Collect the reviews

1. Run the collection script:
```bash
   python <SCRIPTS/TMDB_Script.ipynb>
```
2. The script queries the TMDB API for the English-language user reviews of the 21 films in our study (Spider-Man, Avengers, Captain America, and Iron Man franchises) and saves the raw reviews to `<DATA/marvel_movie_reviews.csv>`.
3. Each row contains the movie title, TMDB movie ID, review ID, review text, author, author rating (if given), and the review's `created_at` timestamp.

### Step 3: Preprocess and compute sentiment

1. Run the preprocessing/sentiment script (or notebook):
```bash
   python <SCRIPTS/TMDB_Lexicon_Sentiment.ipynb>
```
2. This step:
   - Removes duplicate reviews using `review_id` and drops rows missing review text or a date.
   - Adds each film's TMDB release date and computes `days_since_release` = review date − release date.
   - Lowercases the text and removes HTML, URLs, punctuation, and extra whitespace, then splits it into words.
   - Counts matches against our positive and negative word lists (`<DATA/positive_words.txt>`, `<DATA/negative_words.txt>`).
   - Computes the polarity score for each review:
     **polarity = (positive word count − negative word count) / total word count**
3. The output is saved as `OUTPUT/marvel_movie_reviews_lexicon_sentiment.csv`, with columns including `movie`, `franchise`, `days_since_release`, `positive_word_count`, `negative_word_count`, `total_word_count`, and `polarity_score`.

### Step 4: Run the analysis and generate the figures

1. Open `<SCRIPTS/sentiment_time_analysis.ipynb>` in Jupyter.
2. Confirm the first code cell points to the dataset:
```python
   CSV_PATH = file path/"marvel_movie_reviews_lexicon_sentiment.csv"
```
3. Select **Run → Run All Cells** (or **Kernel → Restart & Run All**).
4. The notebook prints the summary statistics and saves six figures to a `figures/` folder:

   | File | What it shows |
   |------|---------------|
   | `1_scatter_lowess.png` | Polarity vs. years since release, with a LOWESS trend line |
   | `2_box_windows.png` | Polarity by timing window (≤1 month, 1 month–1 year, 1–5 years, 5+ years) |
   | `3_by_franchise.png` | Polarity vs. time, one panel per franchise |
   | `4_per_movie_rho.png` | Spearman ρ for each film with ≥8 reviews |
   | `5_timing_hist.png` | Distribution of when reviews were posted |
   | `6_pos_neg_rate.png` | Positive vs. negative word rate by timing window |
5. Move the 'figures/' folder to the 'OUTPUT' folder.
### Step 5: Check that your results match ours

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
