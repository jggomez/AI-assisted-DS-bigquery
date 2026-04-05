## Step 1: Google Cloud Project Provisioning

Before implementing the multimodal workflows and agentic simulations, you must establish a robust cloud foundation. Follow these steps to prepare your environment:

1.  **Create a Google Cloud Project**

      * Navigate to the [Google Cloud Console](https://www.google.com/search?q=https://console.cloud.google.com/).
      * Click on the project dropdown in the top header and select **"New Project"**.
      * Assign a unique name (e.g., `bq-ai-agent-workshop-2026`) and take note of the **Project ID**.

2.  **Enable Billing**

      * Ensure an active billing account is attached to the project. This is mandatory for accessing Vertex AI foundation models and BigQuery ML's advanced features.

3.  **Enable Required APIs**

      * In the search bar, search for and enable the following services:
          * **BigQuery API**
          * **Vertex AI API**
          * **Cloud Storage API**
          * **Compute Engine API** (required for Vertex AI Notebook instances).

4.  **Configure Permissions (IAM)**

      * Navigate to **IAM & Admin \> IAM**.
      * Ensure your user account has the following roles assigned:
          * `BigQuery Admin`
          * `Vertex AI User`
          * `Storage Admin`

5.  **Initialize the Environment**

      * Open the **Cloud Shell** (terminal icon in the top right).
      * Run the following command to set your active project:
        ```bash
        gcloud config set project [YOUR_PROJECT_ID]
        ```
