## Step 8: Clean Up

To avoid incurring unnecessary charges to your Google Cloud account, it is essential to remove the resources created during this workshop. You can either delete the entire project (recommended for a clean slate) or remove the individual BigQuery and Vertex AI components.

---

### Option A: Delete the Entire Project (Fastest)

If you created a specific project for this workshop, deleting the project will immediately stop all billing and remove all associated resources.

1. In the Google Cloud Console, go to the **IAM & Admin > Settings** page.
2. Click **Shutdown**.
3. Enter the **Project ID** and click **Shutdown** again.

---

### Option B: Delete Individual Resources (Granular)

If you are using an existing project and only want to remove the workshop artifacts, run the following commands in a notebook cell or Cloud Shell:

#### 1. Remove BigQuery Tables and Embeddings
```python
# Delete the BigQuery tables
!bq rm --table -f {PROJECT_ID}:housing_dataset.listings
!bq rm --table -f {PROJECT_ID}:housing_dataset.listings_multimodal
!bq rm --table -f {PROJECT_ID}:housing_dataset.home_embeddings
```

#### 2. Remove AI Models (Remote and Trained)
```python
# Delete the remote and clustering models
!bq rm --model -f {PROJECT_ID}:housing_dataset.gemini
!bq rm --model -f {PROJECT_ID}:housing_dataset.kmeans_clustering_model
!bq rm --model -f {PROJECT_ID}:housing_dataset.multimodal_embedding_model
```

#### 3. Remove Connections and Datasets
```python
# Delete the remote connection (adjust location if necessary)
!bq rm --connection --project_id={PROJECT_ID} --location=us ai_connection

# Delete the BigQuery dataset
!bq rm -r -f {PROJECT_ID}:housing_dataset
```
