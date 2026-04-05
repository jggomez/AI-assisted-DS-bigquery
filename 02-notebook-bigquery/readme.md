## Step 2: Setting Up the BigQuery Studio Notebook

In this step, we will transition from the cloud console to your development environment. We are using **BigQuery Studio**, which provides a collaborative, notebook-based interface (similar to Jupyter) directly integrated into the data warehouse.

1.  **Navigate to BigQuery**
    * Open the **Navigation menu** (three horizontal lines in the top-left corner).
    * Scroll down to the **Analytics** section and select **BigQuery**.

2.  **Import the Workshop Notebook**
    * In the **BigQuery Studio** explorer pane (the left-hand sidebar), locate the **+ ADD** button or the dropdown arrow next to your project/region.
    * Hover over **Notebook** and select **Upload**.
    * In the upload dialog, select the **URL** radio button.
    * Paste the following URL:
      ```text
      https://github.com/GoogleCloudPlatform/generative-ai/blob/main/gemini/use-cases/applying-llms-to-data/ai-assisted-data-science/ai-assisted-data-science.ipynb
      ```
    * Set the **Region** to `us-central1`.
    * Click **Upload**.

3.  **Open the Notebook Environment**
    * Once the upload completes, stay in the **Explorer** pane on the left.
    * Click the dropdown arrow next to your **Project ID**.
    * Expand the **Notebooks** folder.
    * Click on `ai-assisted-data-science` to open the file.

4.  **Connect to a Runtime**
    * At the top of the notebook interface, click **Connect** or **Select Runtime**.
    * This will provision a managed Compute Engine instance to execute your Python and SQL cells.
