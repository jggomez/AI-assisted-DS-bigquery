## Step 5: AI-Driven Cluster Interpretation

A common challenge in clustering is translating abstract "Centroid IDs" into actionable business insights. In this section, we use **Generative AI** to act as a Domain Expert (Real Estate Consultant), transforming raw cluster statistics into high-level market summaries.

---

### 1. Aggregate Cluster Statistics
First, we calculate the mathematical "profile" of each cluster. We compute averages for all features, including the AI-extracted `number_windows`, to provide Gemini with a complete data snapshot.

```python
%%bigquery df_cluster_stats --project {PROJECT_ID}

SELECT
  centroid_id AS cluster,
  AVG(price) AS avg_price,
  AVG(sq_ft) AS avg_sq_ft,
  AVG(year_built) AS avg_year_built,
  AVG(number_of_rooms) AS avg_number_of_rooms,
  AVG(number_of_baths) AS avg_number_of_baths,
  AVG(acre_lot) AS avg_acre_lot,
  AVG(property_age) AS avg_property_age,
  AVG(number_windows) AS avg_number_windows,
  COUNT(*) AS cluster_size
FROM
  ML.PREDICT(MODEL `housing_dataset.kmeans_clustering_model`,
    TABLE `housing_dataset.listings_multimodal`)
GROUP BY
  cluster
ORDER BY
  cluster;
```

---

### 2. Formulating the Expert Prompt
We convert the results into a string format and wrap them in a **Persona-based Prompt**. By telling Gemini to act as a "Real Estate Professional," we ensure the output is tailored for a business audience rather than a technical one.

```python
# Convert statistics to a readable string for the LLM
cluster_info = df_cluster_stats.to_string()

# Define the prompt template with clear structural requirements
prompt = f"""
You're a real estate professional. Come up with a description of each cluster based on the data provided.

Clusters Data:
{cluster_info}

For each cluster, please return:
1. A catchy category name (e.g., 'Luxury Estates', 'Starter Homes').
2. Summary statistics: Average price and square footage.
3. Key characteristics: What makes this cluster unique?
4. Target buyer: Who would be most interested in these properties?
"""
```

---

### 3. Generate Narratives with Gemini
Using the **Vertex AI SDK**, we send the data-rich prompt to `gemini-1.5-flash`. This model is optimized for high-speed reasoning and summarization tasks.

```python
from IPython.display import Markdown
from google import genai

# Initialize the Vertex AI Client
client = genai.Client(vertexai=True, project=PROJECT_ID, location="us-central1")

response = client.models.generate_content(
    model="gemini-1.5-flash",
    contents=prompt,
)

# Display the professional market report
display(Markdown(response.text))
```

---

### 4. Visual Verification
To "ground" the AI's descriptions, we retrieve a representative image (sample) from each cluster. This allows us to verify if the visual characteristics match the generated descriptions.

```python
%%bigquery df_example_images --project {PROJECT_ID}

SELECT
  t1.centroid_id AS cluster,
  ANY_VALUE(t2.image_ref).uri AS uri,
  ANY_VALUE(t2.id) AS id
FROM
  ML.PREDICT(MODEL `housing_dataset.kmeans_clustering_model`, 
             TABLE `housing_dataset.listings_multimodal`) AS t1
JOIN
  `housing_dataset.listings_multimodal` AS t2 ON t1.id = t2.id
GROUP BY
  cluster
ORDER BY
  cluster;

# Utility to render GCS images in the notebook
display_images(df_example_images, uri_column="uri", title_column="cluster")
```

* **Contextualization:** We move from "Cluster 0" to "Affordable Family Starter Homes."
* **Data-to-Text Pipeline:** We demonstrate how to feed structured BigQuery ML results directly into a Large Language Model to automate reporting.
* **Multimodal Validation:** By displaying the images alongside the AI's text, we create a feedback loop that validates the entire ML pipeline.
