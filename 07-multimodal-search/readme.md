## Step 7: Multimodal Vector Search & Embeddings

In this final section, we move beyond keyword filtering to **Semantic Search**. By converting images and text into high-dimensional vectors (embeddings), we can find properties that "look like" a sample image or match a conceptual description (e.g., "a house near the ocean") without needing manual metadata tags.

---

### 1. Create the Multimodal Embedding Model
We define a remote model that points to Vertex AI's `multimodalembedding` foundation model. This model is unique because it maps both text and images into the same **vector space**.

```python
%%bigquery --project {PROJECT_ID}

CREATE OR REPLACE MODEL housing_dataset.multimodal_embedding_model
REMOTE WITH CONNECTION `us.ai_connection`
OPTIONS (ENDPOINT = 'multimodalembedding@001');
```

---

### 2. Generate and Index Image Embeddings
We transform our property images into 1408-dimension numeric vectors. To ensure this remains performant at a massive scale, we create a **Vector Index**.

**Generate Embeddings:**
```python
%%bigquery --project {PROJECT_ID}

CREATE OR REPLACE TABLE housing_dataset.home_embeddings AS
SELECT
  id, price, sq_ft, prop_description, image_ref,
  ml_generate_embedding_result AS mm_embedding
FROM ML.GENERATE_EMBEDDING(
  MODEL housing_dataset.multimodal_embedding_model,
  (SELECT *, image_ref AS content FROM housing_dataset.listings_multimodal),
  STRUCT(TRUE AS flatten_json_output)
);
```

**Create Vector Index:**
Using `IVF` (Inverted File Index) and `COSINE` distance to measure similarity.
```python
%%bigquery --project {PROJECT_ID}

CREATE OR REPLACE VECTOR INDEX `house_images_index`
ON housing_dataset.home_embeddings(mm_embedding)
OPTIONS (index_type = 'IVF', distance_type = 'COSINE');
```

---

### 3. Text-to-Image Search (Semantic Query)
We can now search our database using natural language. The query below converts the string "house near the ocean" into a vector and finds the top 3 visually similar matches in BigQuery.

```python
%%bigquery text_to_image_df --project {PROJECT_ID}

SELECT base.image_ref.uri
FROM VECTOR_SEARCH(
  TABLE `housing_dataset.home_embeddings`,
  'mm_embedding',
  (
    SELECT ml_generate_embedding_result
    FROM ML.GENERATE_EMBEDDING(
      MODEL housing_dataset.multimodal_embedding_model,
      (SELECT "house near the ocean" AS content)
    )
  ),
  top_k => 3)
ORDER BY distance ASC;

# Visualize the results
display_images(text_to_image_df)
```

---

### 4. Image-to-Image Search (Visual Similarity)
Finally, we implement "Find more like this." By providing a URI of a test image, BigQuery retrieves the most visually similar properties from the dataset.

```python
%%bigquery image_to_image_df --project {PROJECT_ID}

SELECT base.image_ref.uri
FROM VECTOR_SEARCH(
  TABLE `housing_dataset.home_embeddings`,
  'mm_embedding',
  (
    SELECT ml_generate_embedding_result
    FROM ML.GENERATE_EMBEDDING(
      MODEL housing_dataset.multimodal_embedding_model,
      (SELECT OBJ.FETCH_METADATA(OBJ.MAKE_REF('gs://drw001-data-science-with-notebooks/images/test_image/house_test_image.jpg', 'us.ai_connection')) AS content)
    )
  ),
  top_k => 3)
ORDER BY distance ASC;

# Visualize the results
display_images(image_to_image_df)
```
