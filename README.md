# Kafka Ingestion and PySpark Clustering of Stock Prices

Kafka ingestion of six months of daily prices into 55 per-ticker topics, plus a separate PySpark MLlib K-means stage on one ticker's price series.

<p>
  <a href="https://www.parhamkhoshsolat.com/projects/stock-clustering"><img src="https://img.shields.io/badge/Write--up-portfolio-5dc6ff?style=flat-square&labelColor=0a1422" alt="Write-up: portfolio"></a>
  <img src="https://img.shields.io/badge/2024-team_of_2-5dc6ff?style=flat-square&labelColor=0a1422" alt="2024: team of 2">
</p>

## Overview

A single-broker Kafka 3.3.1 and Zookeeper setup, started from the notebook. A producer sends daily OHLCV data (April to September 2023, from yfinance) for 55 US tickers into one topic per ticker: 6,875 messages (55 tickers × 125 trading days), with 55 topics confirmed by the Kafka CLI. Separately, PySpark MLlib (local mode) runs K-means on one ticker's daily prices: the last ticker downloaded in the producer loop, which by the code's order is TRV (the notebook does not name it).

## What I did

I built the full pipeline: the Kafka setup and producers, and the clustering stage.

## Results

K = 4 was chosen by judgement across the silhouette sweep (K = 2 to 10), the elbow curve and PCA plots for K = 2 to 6; PCA is used only to plot the clusters. Silhouette alone peaks at K = 3 and stays close at K = 4 (about 0.76 and about 0.74). The notebook prints no silhouette or WCSS values; these are read off its plots. The final K = 4 model splits the 125 trading days into clusters of 41, 35, 25 and 24 days.

Limits:

- **The Kafka side is a batch backfill, not a live stream.** Nothing consumes the topics, and Spark does not read from Kafka: the clustering uses the last downloaded ticker's pandas frame from memory. `producer.send` is asynchronous and the "Sent message" lines are printed before delivery is confirmed; per-topic message counts are not read back.
- **The clustering ran on a single ticker's 125 daily rows**, not across all 55, on raw unscaled prices. Open, High, Low and Close correlate at 0.97 to 0.99, so the clusters are price-level bands over time, an exercise in choosing K, not a finding about the market.
- **The K sweeps are unseeded.** The silhouette, elbow and PCA sweeps fit K-means without an explicit seed, so their values can change between runs; only the final K = 4 model is seeded (`setSeed(1)`). The "about 0.74 at K = 4" comes from a sweep fit, not from the final model.
- **Spark runs in local mode** on one machine, so nothing here is distributed in practice.
- The saved outputs are not a clean top-to-bottom run (execution counts jump between the PCA and final-model cells). TA-Lib and scikit-learn are installed and imported but not used.

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
