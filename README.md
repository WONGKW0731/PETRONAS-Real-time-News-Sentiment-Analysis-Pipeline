# Real-time News Sentiment Analysis Pipeline

This project implements a real-time data pipeline to fetch news articles about Petronas, perform sentiment analysis, and store the enriched data for querying and relationship analysis. The pipeline leverages Kafka for streaming, Spark for distributed processing, and stores results in MongoDB and Neo4j.

## Project Overview

The system is designed to track public sentiment, monitor brand reputation, and observe sentiment trends over time. It ingests articles from the **NewsAPI**, processes them in real-time, analyzes the sentiment of the content, and stores the structured results for advanced querying and graph-based relationship analysis.

### Key Features

* **Data Ingestion:** Fetches news articles from NewsAPI and streams them into a Kafka topic.
* **Web Scraping:** Scrapes and cleans the full content of articles from their URLs in a distributed manner using PySpark.
* **Real-time Processing:** Uses Spark Structured Streaming to process articles from Kafka as they arrive.
* **Sentiment Analysis:** Applies a pre-trained sentiment analysis model to each sentence to classify it as positive, negative, or neutral.
* **Data Enrichment:** Enriches the data by extracting named entities (people, organizations, locations), categories, and other metadata using spaCy and regex.
* **Data Storage:**
    * **MongoDB:** Stores the final, enriched data in a document-oriented model for flexible querying and reporting.
    * **Neo4j:** Models the data as a graph database to analyze relationships between entities (e.g., people mentioned in articles, organizations, and sentiment).

## System Architecture & Workflow

The pipeline follows these main stages:

1.  **Data Ingestion:** A `NewsAPIFetcher` continuously polls the NewsAPI for new articles related to "Petronas" and publishes the article metadata to a Kafka topic (`news-articles-content`).
2.  **Real-time Sentiment Analysis:** A `StreamingProcessor` subscribes to the Kafka topic. Using Spark Structured Streaming, it processes each incoming article in micro-batches.
3.  **Text Cleaning & Prediction:** For each article, the content is cleaned, split into sentences, and a `SentimentPredictor` model classifies the sentiment of each sentence.
4.  **Data Validation & Enrichment:** The results are collected into a static DataFrame where a `DataValidator` checks for quality and a `DataEnricher` extracts additional metadata and named entities.
5.  **Data Storage & Analysis:** The final enriched data is loaded into both MongoDB for reporting and Neo4j for relationship analysis.

![Workflow Diagram](https://i.imgur.com/your-workflow-diagram.png) 
*(You can upload the workflow diagram from your report to an image hosting service like Imgur and link it here)*

## Technology Stack

* **Data Source:** NewsAPI
* **Streaming:** Apache Kafka
* **Processing:** Apache Spark (PySpark)
* **Web Scraping:** Requests, BeautifulSoup
* **NLP/Sentiment Analysis:** Hugging Face Transformers, spaCy, NLTK
* **Databases:**
    * MongoDB (for Reporting)
    * Neo4j (for Relationship Analysis)
* **Language:** Python

## Setup and Installation

1.  **Prerequisites:**
    * Python 3.8+
    * Java 8+
    * Apache Spark
    * Apache Kafka
    * MongoDB
    * Neo4j

2.  **Clone the repository:**
    ```bash
    git clone <your-repository-url>
    ```

3.  **Install Python dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

4.  **Download NLP Models:**
    ```bash
    python -m spacy download en_core_web_sm
    ```

5.  **Environment Variables:**
    Set your NewsAPI key as an environment variable:
    ```bash
    export NEWSAPI_KEY='your_api_key_here'
    ```

6.  **Run the Pipeline:**
    Execute the main driver script to start the data ingestion and processing pipeline. Ensure Kafka, Spark, MongoDB, and Neo4j services are running.

## Querying and Reporting

The final data can be queried directly from MongoDB and Neo4j. The report (`G 1-3_TeamReport (1).pdf`) contains examples of analytical queries, such as:
* Daily and monthly sentiment trends.
* Top categories associated with different sentiments.
* Articles with mixed sentiment.
* Co-mentions of people and organizations.
