# AI-Assisted Data Science with Multimodal BigQuery

This repository contains an advanced workshop focused on implementing **Multimodal Data Science** workflows using the **Google Cloud (BigQuery + Vertex AI)** ecosystem.

Unlike traditional ML approaches, this lab demonstrates how to integrate Large Language Models (LLMs) and vector search directly into a SQL-native data platform. This allows for the simultaneous processing of structured data (prices, square footage) and unstructured data (property images).

-----

## Workshop Objectives

By completing this lab, you will be able to:

  * **Orchestrate Remote Models:** Configure secure connections between BigQuery and Vertex AI models (Gemini 1.5 Flash).
  * **Multimodal Feature Engineering:** Extract visual features from images (e.g., "near water," "number of windows") using standard SQL.
  * **Advanced Clustering:** Train K-Means models in BigQuery and integrate results with the Vertex AI Model Registry.
  * **GenAI Interpretation:** Automate the creation of buyer personas and market summaries using AI Agents.
  * **Vector Search:** Implement semantic "Text-to-Image" and "Image-to-Image" search capabilities.

-----

## Technology Stack

| Component | Function |
| :--- | :--- |
| **BigQuery Studio** | Collaborative notebook environment (SQL + Python). |
| **Gemini 1.5 Flash** | Feature extraction and natural language narrative generation. |
| **Vertex AI SDK** | Orchestration of embedding models and agentic workflows. |
| **BigQuery ML (BQML)** | In-warehouse K-Means clustering model training. |
| **Vector Search** | Indexing and cosine similarity search for high-dimensional data. |

-----

## 📂 Repository Structure

The workshop is organized into the following logical steps:

1.  **[Step 1: Provisioning](https://www.google.com/search?q=./docs/step1.md):** GCP project setup and API configuration.
2.  **[Step 2: Environment](https://www.google.com/search?q=./docs/step2.md):** Importing the notebook into BigQuery Studio.
3.  **[Step 3: Enrichment](https://www.google.com/search?q=./docs/step3.md):** Feature engineering using Gemini to "read" visual assets.
4.  **[Step 4: Modeling](https://www.google.com/search?q=./docs/step4.md):** Building and evaluating the clustering model.
5.  **[Step 5: Interpretation](https://www.google.com/search?q=./docs/step5.md):** Generating business insights with Generative AI.
6.  **[Step 6: Agentic DS](https://www.google.com/search?q=./docs/step6.md):** Automating the pipeline via the Data Science Agent.
7.  **[Step 7: Vector Search](https://www.google.com/search?q=./docs/step7.md):** Implementing multimodal semantic search.

-----

## 🚀 Getting Started

1.  Clone this repository.
2.  Follow the instructions in **Step 1** to prepare your Google Cloud environment.

-----

**Developed by:** Google Team
