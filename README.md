# Stock Market Clustering Pipeline

![Python](https://img.shields.io/badge/python-3.8%2B-yellow)
![Apache Kafka](https://img.shields.io/badge/Apache-Kafka-red)
![PySpark](https://img.shields.io/badge/Apache-PySpark-orange)

Real-time data pipeline and clustering analysis for 50+ US stocks. Apache Kafka handles streaming from Yahoo Finance; PySpark processes and transforms the data; K-means identifies behavioral clusters for portfolio analysis.

## Team

- Parham Khosh Solat
- Farshad Farahtaj

## How it works

Six months of price and volume data (April–September 2023) for 50+ stocks including AAPL, GOOGL, MSFT, and TSLA stream through individual Kafka topics — one per ticker. PySpark processes the stream and builds feature vectors for clustering.

Optimal cluster count (K=4) was selected through silhouette scoring and the elbow method. PCA reduces dimensions for 3D visualization of cluster separation.

The four clusters map to distinct behavioral patterns: low-volatility stable stocks, growth-oriented momentum stocks, high-volatility speculative positions, and cyclical plays. Each suggests a different portfolio role.

## Tech Stack

- Apache Kafka — streaming and topic management per ticker
- Apache Spark / PySpark — distributed processing
- PySpark ML — K-means, PCA
- Yahoo Finance API — historical data source
- Matplotlib, Seaborn — visualization
- TA-Lib — technical indicators

## Setup

```bash
git clone https://github.com/Alirezakhoshsolat/stock-clustering-pipeline.git
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
