# Frequent Itemset Mining on Movie Casts

Market-basket analysis of the [IMDB Top 1000 Movies & TV Shows](https://www.kaggle.com/datasets/harshitshankhdhar/imdb-dataset-of-top-1000-movies-and-tv-shows) dataset: each movie is treated as a basket, and its four credited lead actors (`Star1`-`Star4`) as items. The goal is to mine **frequent itemsets of co-occurring actors** -- groups of actors who recur together across multiple films -- using the **A-Priori algorithm implemented from scratch**.

Project for the *Algorithms for Massive Data* course, Master in Data Science for Economics, Università degli Studi di Milano (Prof. Dario Malchiodi).

## Repository structure

```
imdb_actor_apriori.ipynb       Main notebook: full pipeline, from data download to results
report.tex                     Project report (LaTeX source)
report.pdf                     Project report (compiled)
requirements.txt                Python dependencies
basket_size_distribution.png    Figure used in the report
scalability_experiment.png      Figure used in the report
data/                           Downloaded dataset (gitignored, fetched at runtime)
```

## Setup

1. Install dependencies:
   ```
   pip install -r requirements.txt
   ```
2. Get a Kaggle API credential and place it locally (never committed to this repo):
   - either `~/.kaggle/access_token` (Kaggle's newer token, from Kaggle account settings -> API)
   - or `~/.kaggle/kaggle.json` (the older username/key pair)
3. Open `imdb_actor_apriori.ipynb` in VS Code (with the Python + Jupyter extensions) or Jupyter, and run all cells top to bottom. The first code cell downloads the dataset automatically via the Kaggle CLI.

No credentials of any kind live in the notebook or this repository -- the Kaggle CLI reads them from the local file above.

## What the notebook does

1. Downloads the dataset via the Kaggle API.
2. Cleans the `Star1`-`Star4` columns and builds one basket (a set of actors) per movie.
3. Runs exploratory analysis on basket-size distribution and actor-pair sparsity.
4. Implements **A-Priori from scratch**: frequent singletons, then iterative candidate generation (join + downward-closure pruning) and counting, level by level.
5. Reports frequent itemsets and derives association rules (confidence, lift).
6. Runs a scalability experiment, re-mining the data at several sample sizes and measuring runtime.
7. Cross-checks the from-scratch results against the `mlxtend` library's `apriori` implementation.

`USE_FULL_DATA` and `SAMPLE_SIZE` (configured in the first code cell) control whether the pipeline runs on the full dataset or a reproducible random subsample.

## Key results

On the full dataset (1000 movies), the algorithm finds **299 frequent itemsets** (271 actors, 25 pairs, 3 triples), all verified to match `mlxtend` exactly. The three frequent triples correspond to the lead casts of three film franchises: Harry Potter, the original Star Wars trilogy, and The Lord of the Rings. Full methodology, experiments, and discussion are in `report.pdf`.
