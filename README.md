# Kafka Ingestion and PySpark Clustering of Stock Prices

Kafka ingestion of six months of daily prices into 55 per-ticker topics, plus a separate PySpark MLlib K-means stage on one ticker's price series.

<p>
  <a href="https://www.parhamkhoshsolat.com/projects/stock-clustering"><img src="https://img.shields.io/badge/Write--up-portfolio-5dc6ff?style=flat-square&labelColor=0a1422" alt="Write-up: portfolio"></a>
  <img src="https://img.shields.io/badge/2024-team_of_2-5dc6ff?style=flat-square&labelColor=0a1422" alt="2024: team of 2">
</p>

## Overview

A single-broker Kafka 3.3.1 and Zookeeper setup, started from the notebook. Producers send daily OHLCV data (April to September 2023, from yfinance) for 55 US tickers into one topic per ticker: 6,875 messages (55 tickers × 125 trading days). Separately, PySpark MLlib (local mode) runs K-means on one ticker's daily prices.

## What I did

I built the full pipeline: the Kafka setup and producers, and the clustering stage.

## Results

K = 4 was chosen from silhouette and elbow plots; PCA is used only to plot the clusters. Two honest limits:

- **The Kafka side is a batch backfill, not a live stream.** Nothing consumes the topics, and Spark does not read from Kafka.
- **The clustering ran on a single ticker's 125 daily rows**, not across all 55, so the clusters are an exercise in choosing K, not a finding about the market.

## Run it

```bash
git clone https://github.com/parhamkhoshsolat/stock-clustering-pipeline.git
cd stock-clustering-pipeline
pip install -r requirements.txt
jupyter notebook "Stock Clustering Analysis.ipynb"
```

Kafka setup is automated inside the notebook.

## Project layout

```
├── Stock Clustering Analysis.ipynb   # Kafka setup, producers, clustering
└── requirements.txt
```

## Team & course

Hardware and Software for Big Data, University of Naples Federico II. Supervisor: Prof. Giancarlo Sperlì. Team: Parham Khosh Solat, Farshad Farahtaj.

## License

MIT. See [`LICENSE`](LICENSE).

---

Part of my portfolio: [parhamkhoshsolat.com](https://www.parhamkhoshsolat.com)
