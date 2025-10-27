# Analyze ETFs with Blue Morpho

Information on ETFs such as fees, structure, or ESG criteria is often buried in unstructured documents. Blue Morpho lets you model the key data points once and extract them consistently across all documents, making comparison and analysis easy.

In this tutorial, we’ll answer questions about three ETFs using their KID and Factsheet documents.

## Step 1: Create a Project
Every workflow in Blue Morpho begins with a project. A project organizes your collections, ontologies, extractions, and knowledge bases so all related work stays in one place.

1. Go to **Projects**.  
2. Click **New Project**.  
3. Enter a name (e.g. “ETF Analysis”) and a short goal (e.g. “Automate analysis of multiple ETFs”).  
4. Click **Create**.

## Step 2: Create a Collection
A collection is a set of documents analyzed with the same ontology. You can upload PDFs (automatically parsed into text) or TXT files (ingested directly). Parsing time depends on file size and number of documents.

1. Go to **Collections**.  
2. Click **New Collection**.  
3. Enter a name (e.g. “ETF Documents”) and a description (e.g. “Factsheets and KIDs for ETFs”).  
4. Upload the [sample PDFs](https://github.com/getbluemorpho/blue-morpho/tree/main/docs/assets/tutorial-analyze-ETFs) provided for this tutorial.

5. Click **Create**.

> **Note:** The dataset includes both KID and Factsheet PDFs for three ETFs. These are for demonstration only and not for financial use.

## Step 3: Create an Ontology
An ontology defines the structure of your data — what entities exist, their properties, and how they relate. Think of it as a blueprint for organizing knowledge extracted from your documents.

ETF documents include hundreds of data points, from detailed portfolio holdings to complex derivative exposures. In this tutorial, we’ll focus on a few essential elements to keep the model clear, practical, and easy to follow.

### Entities
- **ETF:** Name, ISIN, management company, benchmark index, replication method, ESG approach, fees, SRRI, and trading details.  
- **Sector:** Basic investment categories (e.g. “Health Care”, “Technology”).  
- **SectorExposure:** Links ETFs to sectors, with attributes for *date* and *weight percentage* (e.g. “15.3% in Healthcare”).

### Relationships
- ETF → has_sector_exposure → SectorExposure  
- SectorExposure → references → Sector  

This allows queries such as: *“What’s the average technology exposure across all ETFs?”*

### Create the Ontology
You can create an ontology manually or use the ontology assistant. For this tutorial, we’ve already prepared one for you to import.

1. Go to **Ontologies**.  
2. Click **New Ontology**.  
3. Enter a name and description (e.g. “ETF analysis ontology”).  
4. Choose **Import from YAML file** and upload the provided [ontology template](https://github.com/getbluemorpho/blue-morpho/blob/main/docs/assets/tutorial-analyze-ETFs/ETF_analysis_ontology.yaml).  
5. Click **Create**.

### Explore your Ontology
After importing, you’ll see your ontology as a list of entities and relationships. Review the entities and their properties, especially those related to ETFs.

## Step 4: Run an Extraction
Now let’s extract data from your ETF documents based on your ontology.

1. Go to **Extraction Runs**.  
2. Click **New Run**.  
3. Enter a name (e.g. “ETF Extraction”) and a description (e.g. “Extract key ETF information from Factsheets and KIDs”).  
4. Select your ontology and collection.  
5. Click **Create**.  
6. Skip **Chunking** (not needed for short documents).  
7. Add an **Extraction step** with default settings and click **Extract**.

### Review your Extraction Results
The **Results** view lists all extracted ETF information from your Factsheets and KIDs, along with related sectors and *SectorExposure* entities.

For example, search for **“iShares Core MSCI World”** to compare data extracted from its KID and Factsheet.

Use this step to verify that Blue Morpho correctly extracted the information, created entities and relationships, and adjust the ontology if needed.

When satisfied, scroll back up and click **Deduplication** (optional).

## Deduplicate Extraction Results
Deduplication combines identical entities extracted from multiple sources, preventing duplicates in your knowledge base.

You’ll likely see duplicates for:
- **ETFs**, since information comes from both Factsheets and KIDs  
- **Sectors**, since a single sector may appear in multiple ETFs  

To deduplicate:
1. Click **Add class to deduplicate entities**.  
2. Select **ETF** and **Sector**.  
3. Click **Generate rules**.  
4. Click **Deduplicate**.

In this case, the process is straightforward and requires no additional configuration.

### Review your Deduplication Results
Scroll down to review the results.  
The **“iShares Core MSCI World”** ETF now appears as a single entity that combines properties from both source documents.  

For **Sector** entities, variations such as *“Financial services”* and *“Finance”* are now merged into a standardized *“Financials”* sector, enabling consistent analysis across all ETFs.

You can guide the process by specifying preferred final names in the **Guidelines for detecting duplicates**.  
*Example: “Deduplicate all financial services–related sectors into a single sector named ‘Financial services.’”*

## Step 5: Create a Knowledge Base
Once extraction and deduplication look correct, turn them into a Knowledge Base so you can query them.

1. In **Extraction Results**, click **Create Knowledge Base**.  
2. Add a name (e.g. “ETF Analysis KB”) and a description.  
3. Click **Create**.

## Step 6: Query your Knowledge Base
With your Knowledge Base ready, you can now ask questions in plain language. Blue Morpho translates each query into a structured search based on your ontology and the data extracted from your ETF Factsheets and KIDs.

1. Go to **Knowledge Bases**.  
2. Select your Knowledge Base.  
3. Click **Ask** to start a chat with it.

### Example 1 – Find ETFs
Quickly identify which ETFs match your specific criteria, even if you don’t use the exact terminology.

**Example query:**  
*Show me the ETF with the best environmental characteristics, low risk, and minimal management requirements.*

### Example 2 – Analyze Portfolio Sector Exposure
Compare the sector exposure of your selected ETFs using the latest available data.

**Example query:**  
*Compare sector exposure across my ETFs.*

### Example 3 – Compare Fees Between ETFs
Compare cost structures across all ETFs in your portfolio or selection.

**Example query:**  
*Compare total fees over 5 years for an initial investment of €10,000, followed by monthly contributions of €500 and a full exit after 5 years, assuming a 10% annual return across all ETFs.*

**Format as a table:**  
`ETF | Entry fees | Transaction fees | Other fees | Exit fee`

You can also ask follow-up questions to explore details:  
*Show me the detailed fee structure for BNP PARIBAS EASY MSCI EMU SRI PAB.*

## Next Steps
You now have a working ETF analysis workflow.

You can:
- Add new ETF Factsheets and KIDs to expand your analysis.  
- Extend the ontology with new dimensions such as geographic exposure, equity exposure, currency risk, or protection mechanisms.  

Continue exploring Blue Morpho with these next tutorials:
- **Analyze other financial products:** Adapt the ontology to study corporate bonds, mutual funds, or other instruments.  
- **Consolidate investment portfolios:** Follow the [Investment portfolios tutorial](https://docs.getbluemorpho.com/tutorials/consolidate%20investment%20portfolios/) to combine
financial reports into a single knowledge base. 
- **Integrate through [MCP](https://docs.getbluemorpho.com/product/core%20concepts/5.%20setup%20blue%20morpho%20mcp/):** Query your Knowledge Base directly from your own environment.
