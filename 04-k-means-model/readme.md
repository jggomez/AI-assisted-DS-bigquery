## Step 4: Segmentation with K-Means Clustering

Now that we have a rich, multimodal dataset, we will use **Unsupervised Machine Learning** to discover hidden patterns in the housing market. By grouping properties based on similarity, we can identify distinct market segments (e.g., luxury waterfronts vs. affordable historic homes).

---

### 1. Model Training & Vertex AI Registration

We will use the `KMEANS` algorithm directly in BigQuery. A key innovation here is the `model_registry='VERTEX_AI'` option, which automatically versions and tracks this model in the central Vertex AI dashboard for enterprise-grade management.

```python
%%bigquery --project {PROJECT_ID}

CREATE OR REPLACE MODEL `housing_dataset.kmeans_clustering_model`
OPTIONS(model_type='KMEANS', 
        num_clusters=3,
        model_registry = 'VERTEX_AI', 
        VERTEX_AI_MODEL_ID = 'housing_clustering') AS
SELECT
  price,
  sq_ft,
  year_built,
  number_of_rooms,
  number_of_baths,
  acre_lot,
  property_age,
  near_water,
  number_windows
FROM
  `housing_dataset.listings_multimodal`;
```

---

### 2. Model Evaluation and Prediction

Before using the clusters, we must evaluate their quality using the **Davies-Bouldin Index** (where a lower value indicates better-defined clusters). Then, we will map every listing to its respective cluster ID.

**Evaluate the Model:**
```python
%%bigquery --project {PROJECT_ID}

SELECT * FROM ML.EVALUATE(MODEL `housing_dataset.kmeans_clustering_model`);
```

**Generate Predictions:**
We save the results into a Pandas DataFrame (`df`) for immediate visualization.
```python
%%bigquery df --project {PROJECT_ID}

SELECT
  CENTROID_ID AS cluster,
  * EXCEPT(CENTROID_ID)
FROM
  ML.PREDICT(
    MODEL `housing_dataset.kmeans_clustering_model`,
    TABLE `housing_dataset.listings_multimodal`
);
```

---

### 3. Visualize and Interpret Clusters

Data science isn't complete without interpretation. We will use **Seaborn** and **Matplotlib** to compare how the AI-extracted features (like `near_water`) influence the clustering logic.

**Numerical Distribution (Price & Age):**
```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.set_style("whitegrid")
plt.figure(figsize=(14, 6))
plt.suptitle("Numeric Variable Distributions by Cluster", fontsize=16)

# Price Distribution
plt.subplot(1, 2, 1)
sns.boxplot(x="cluster", y="price", data=df, hue="cluster", palette="tab20", legend=False)
plt.title("Price Distribution per Cluster")

# Property Age Distribution
plt.subplot(1, 2, 2)
sns.boxplot(x="cluster", y="property_age", data=df, hue="cluster", palette="tab20", legend=False)
plt.title("Property Age Distribution per Cluster")

plt.tight_layout(rect=[0, 0, 1, 0.96])
plt.show()
```

**Categorical Distribution (AI-Generated Features):**
```python
# Analyze the impact of the AI-detected 'near_water' feature
water_crosstab = pd.crosstab(df["cluster"], df["near_water"])
water_proportions = water_crosstab.div(water_crosstab.sum(axis=1), axis=0)

water_proportions.plot(kind="bar", stacked=True, figsize=(10, 6), colormap="tab20")
plt.title("Proportion of Properties Near Water within Each Cluster", fontsize=16)
plt.xlabel("Cluster ID")
plt.ylabel("Proportion")
plt.xticks(rotation=0)
plt.legend(title="Is Near Water?", labels=["No", "Yes"])
plt.show()
```
