# AI-Assisted Data Science with Multimodal BigQuery

This repository contains an advanced workshop focused on implementing **Multimodal Data Science** workflows using the **Google Cloud (BigQuery + Vertex AI)** ecosystem.

Unlike traditional ML approaches, this lab demonstrates how to integrate Large Language Models (LLMs) and vector search directly into a SQL-native data platform. This allows for the simultaneous processing of structured data (prices, square footage) and unstructured data (property images).

-----

## Workshop Objectives

By completing this lab, you will be able to:

  * **Orchestrate Remote Models:** Configure secure connections between BigQuery and Vertex AI models (Gemini 2.5 Flash).
  * **Multimodal Feature Engineering:** Extract visual features from images (e.g., "near water," "number of windows") using standard SQL.
  * **Advanced Clustering:** Train K-Means models in BigQuery and integrate results with the Vertex AI Model Registry.
  * **GenAI Interpretation:** Automate the creation of buyer personas and market summaries using AI Agents.
  * **Vector Search:** Implement semantic "Text-to-Image" and "Image-to-Image" search capabilities.

-----

## Technology Stack

| Component | Function |
| :--- | :--- |
| **BigQuery Studio** | Collaborative notebook environment (SQL + Python). |
| **Gemini 2.5 Flash** | Feature extraction and natural language narrative generation. |
| **Vertex AI SDK** | Orchestration of embedding models and agentic workflows. |
| **BigQuery ML (BQML)** | In-warehouse K-Means clustering model training. |
| **Vector Search** | Indexing and cosine similarity search for high-dimensional data. |

-----

## Repository Structure

The workshop is organized into sequential modules. Each directory contains a specific `README.md` with detailed technical instructions and code snippets:

* **01-GCP-Configuration:** Initial project provisioning, billing setup, and API enablement (BigQuery, Vertex AI, Gemini).
* **02-notebook-bigquery:** Setup of the BigQuery Studio environment and importing the foundational Colab Enterprise notebook.
* **03-Feature-Engineering:** Data cleaning and multimodal enrichment using Gemini 1.5 Flash to extract features from property images via SQL.
* **04-k-means-model:** Training and evaluating an unsupervised K-Means clustering model directly in BigQuery ML with Vertex AI Model Registry integration.
* **05-cluster-gemini:** Using Generative AI to transform abstract cluster statistics into professional real estate market descriptions.
* **06-data-science-agent:** Exploring "Agentic Data Science" by using natural language prompts to automate Python-based modeling and visualization.
* **07-multimodal-search:** Implementing semantic and visual search using multimodal embeddings and BigQuery Vector Search.
* **08-cleaning:** Best practices for resource governance and environment cleanup to ensure a responsible AI lifecycle.

-----

## 🚀 Getting Started

1.  Clone this repository.
2.  Follow the instructions in **Step 1** to prepare your Google Cloud environment.

-----

**Developed by:** Google Team
