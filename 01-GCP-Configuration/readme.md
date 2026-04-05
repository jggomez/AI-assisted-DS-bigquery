## Step 1: Google Cloud Project Provisioning & API Configuration

Before implementing the multimodal workflows and agentic simulations, you must establish a robust cloud foundation. Follow these steps to prepare your environment:

1.  **Create a Google Cloud Project**
    * Navigate to the [Google Cloud Console](https://console.cloud.google.com/).
    * Click on the project dropdown in the top header and select **"New Project"**.
    * Assign a unique name (e.g., `bq-ai-agent-workshop-2026`) and take note of the **Project ID**.

2.  **Enable Billing**
    * Ensure an active billing account is attached to the project. This is mandatory for accessing Vertex AI foundation models and BigQuery ML's advanced features.

3.  **Enable Required APIs**
    * Open the **Cloud Shell** (terminal icon in the top right of the console).
    * Execute the following command to enable the core services for BigQuery, Vertex AI, and Gemini:

    ```bash
    gcloud services enable bigquery.googleapis.com \
                           bigqueryunified.googleapis.com \
                           cloudaicompanion.googleapis.com \
                           aiplatform.googleapis.com
    ```

4.  **Configure Permissions (IAM)**
    * Navigate to **IAM & Admin > IAM**.
    * Ensure your user account has the following roles assigned:
        * `BigQuery Admin`
        * `Vertex AI User`
        * `Storage Admin`

5.  **Initialize the Environment**
    * In the **Cloud Shell**, run the following command to ensure you are working within the correct project:
    ```bash
    gcloud config set project [YOUR_PROJECT_ID]
    ```
