# Stock Market Clustering Pipeline

![Python](https://img.shields.io/badge/python-3.8%2B-yellow)
![Apache Kafka](https://img.shields.io/badge/Apache-Kafka-red)
![PySpark](https://img.shields.io/badge/Apache-PySpark-orange)

Market-data ingestion and unsupervised clustering. A Kafka and Zookeeper cluster ingests six months of daily price data from Yahoo Finance across 55 per-ticker topics; a separate PySpark MLlib stage clusters a price series with K-means and PCA.

## Team

- Parham Khosh Solat
- Farshad Farahtaj

## How it works

Six months of daily price and volume data (April to September 2023) for 55 tickers including AAPL, GOOGL, MSFT and TSLA were pushed through individual Kafka topics, one per ticker, 7,028 messages in total.

The clustering stage is separate. It runs in PySpark MLlib with K-means and PCA, and the cluster count was chosen by silhouette scoring and the elbow method rather than picked by hand.

## Honest scope

Two things this project does not do, which the code makes clear on inspection:

**The Kafka side is a batch backfill, not a live stream.** Producers write to the topics, but nothing consumes them. There is no Structured Streaming job. The value here is the ingestion infrastructure being stood up correctly, not a running real-time system.

**The clustering ran on a single ticker's 125 daily rows**, not across the 55. A data-overwrite bug in the loading step meant each ticker's frame replaced the last, so the clustering input was one series rather than a cross-section. That invalidates any market-segmentation reading of the output, and an earlier version of this README described four behavioural stock archetypes that the data cannot support. That claim has been removed.

## Tech Stack

- Apache Kafka — streaming and topic management per ticker
- Apache Spark / PySpark — distributed processing
- PySpark ML — K-means, PCA
- Yahoo Finance API — historical data source
- Matplotlib, Seaborn — visualization

## Setup

```bash
git clone https://github.com/parhamkhoshsolat/stock-clustering-pipeline.git
cd stock-clustering-pipeline
pip install -r requirements.txt
jupyter notebook "Stock Clustering Analysis.ipynb"
```

Kafka setup is automated inside the notebook.

## Course

Big Data Technologies — University of Naples Federico II
Supervisor: Prof. Giancarlo Sperli

## License

MIT
