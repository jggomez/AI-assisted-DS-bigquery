## Step 3: Feature Engineering & Multimodal Enrichment

In this stage, we transition from raw data to **AI-ready features**. We will perform two critical operations: traditional data cleaning via SQL and advanced multimodal enrichment using **Gemini** to "see" and describe the property images.

---

### 1. Clean, Filter, and Reference Unstructured Data

We will create a refined table from the raw dataset. This step performs three key actions:
* **Filtering:** Narrowing the scope to listings currently 'For Sale'.
* **Feature Derivation:** Calculating `property_age` dynamically.
* **Unstructured Data Linking:** Using `OBJ.MAKE_REF` to create an **Object Reference**, allowing BigQuery to point directly to the images stored in Google Cloud Storage.

```python
%%bigquery --project {PROJECT_ID}

CREATE OR REPLACE TABLE `housing_dataset.listings` AS
SELECT
  *,
  EXTRACT(YEAR FROM CURRENT_DATE()) - year_built AS property_age,
  OBJ.FETCH_METADATA(OBJ.MAKE_REF(house_uri, 'us.ai_connection')) AS image_ref
FROM
  `housing_dataset.listings`
WHERE sale_status = 'For Sale';
```

---

### 2. Register the Remote AI Model

To interact with LLMs like Gemini directly from BigQuery, we define a **Remote Model**. This acts as a secure gateway to the Vertex AI API without moving your data out of the BigQuery environment.

```python
%%bigquery --project {PROJECT_ID}

CREATE OR REPLACE MODEL `housing_dataset.gemini`
REMOTE WITH CONNECTION `us.ai_connection`
  OPTIONS(ENDPOINT = 'gemini-2.5-flash');
```

---

### 3. Multimodal Enrichment via `AI.GENERATE_TEXT`

Now, we use the Gemini model to analyze the visual content of our listings. We provide a prompt and a specific **output schema** so the AI returns structured data that BigQuery can immediately store in a table.

We are extracting three new features directly from the images:
1.  **near_water** (Boolean)
2.  **number_windows** (Integer)
3.  **prop_description** (String)

```python
%%bigquery --project {PROJECT_ID}

CREATE OR REPLACE TABLE `housing_dataset.listings_multimodal` AS (
SELECT 
  id,
  price,
  sq_ft,
  year_built,
  city,
  zipcode,
  number_of_rooms,
  number_of_baths,
  acre_lot,
  property_age,
  near_water,
  number_windows,
  prop_description,
  image_ref
FROM AI.GENERATE_TEXT(
  MODEL `housing_dataset.gemini`,
  (
    SELECT 
      CONCAT(
        'Analyze the following image to find whether it is near a body of water, ',
        'the number of windows, and give a brief description of the property.'
      ) AS prompt,
      image_ref
    FROM `housing_dataset.listings`
  ),
  STRUCT(
     0.2 AS temperature,
     TRUE AS flatten_json_output,
     "near_water BOOL, number_windows INT64, prop_description STRING" AS output_schema
  )
));
```
