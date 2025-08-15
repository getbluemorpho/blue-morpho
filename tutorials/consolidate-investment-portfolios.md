# Consolidate and Analyze Your Investment Portfolios Using Blue Morpho

If you have different investment account types (IRAs, 401(k)s, taxable accounts) or use different brokers, you know how tedious it is to get a complete picture of your holdings. This tutorial shows you how to use Blue Morpho to:
- Consolidate identical holdings across multiple portfolios for a unified view
- Perform aggregations, calculations, and analysis
- Create interactive dashboards to manage your holdings

This use case demonstrates the strength of our approach, as these results cannot be obtained using any state-of-the-art model out of the box.

> **Similar Use Cases**
> This approach works for any use case where you need to structure and homogenize data for structured queries. Similar examples include:
> - **Ledger consolidation:** Extract journal entries, accounts, and amounts from PDF exports to unify multiple accounting systems
> - **Portfolio management for Private Equity:** Create portfolio management applications and automate reporting to LPs
> - **Asset management & maintenance:** Merge asset registries from different plants or fleets to enable fleet-wide analytics
> - **Healthcare records analysis:** Structure patient information, diagnoses, procedures, and billing codes from PDF medical records for analysis

## Step 1: Upload Your Portfolio Statements

Go to the **Collections** section and create a new collection containing your investment statements (PDFs). For this tutorial, we'll use sample statements from Interactive Brokers. The statements cover three different brokerage accounts:
- A traditional IRA
- A Roth IRA  
- A regular taxable account

You can download them [here](https://github.com/getbluemorpho/blue-morpho/tree/main/assets/Interactive%20Brokers%20sample%20statements).

Upon upload, **Blue Morpho** will automatically parse these documents, transforming them into raw text while preserving table structures to feed them to LLMs in the next steps.

## Step 2: Define Your Ontology

When consolidating multiple portfolios, key concepts (ontology classes) include `Accounts`, `Holdings`, and `Financial Instruments`.

Navigate to the **Ontologies** section and create a new ontology. You can name it "consolidate portfolios". After the creation, click on "New version": you can either use the editor or download our [example ontology in YAML format](https://github.com/getbluemorpho/blue-morpho/blob/main/assets/consolidate%20portfolios.yaml) and import it directly into **Blue Morpho**.

You should see the following classes: 

And the following relationships:

If desired, you can refine the ontology further by modeling `Transactions`, `Dividends`, and other financial concepts.

## Step 3: Run an Extraction

Navigate to the **Extraction Runs** section and start a new run using your ontology and document collection.

### 3.1 Chunking
This step is not needed for portfolio statements (it's reserved for very long documents). Navigate directly to **Extraction**. 

### 3.2 Extraction
The default settings work well and there is no need to configure advanced parameters. Click "Extract" to perform the extraction.

At this stage, you should have extracted 3 `Accounts`, 192 `Financial Instruments`, 185 `Holdings` and 7 `Cash Balances`.

As you can see in the results section, relations have also been extracted. For example, this particular `Holding` is linked with one `Financial Instrument` and one `Account` (you can click on them to see their details):

<details>
<summary>Advanced considerations: How Blue Morpho Handles Different Broker Data Formats</summary>
<br>
Different brokers format data inconsistently:
- Some classify interest payments or dividends as transactions, others don't
- Some show sell transactions as negative quantities, others as positive
- Date formats, currency symbols, and field names vary across platforms

**What you can do:**
 - **Use descriptive class definitions**: For example, if you only want stock/bond transactions (excluding interest payments or dividends), state this in your `Transaction` class description
 - **Use descriptive property definitions**: For example, if you want sell transactions as positive quantities, specify that all `quantity` and `cost` values should be positive numbers and add a `transaction_type` property that is either "sell" or "buy".

Blue Morpho will automatically transform inconsistent broker data into clean, standardized entities that follow your exact specifications. 

</details>

### 3.3 Deduplication
This critical step consolidates duplicate entities across your multiple accounts. For example, Amazon (AMZN) stock is held in all three accounts, so it was extracted three times as separate `Financial Instrument` entities. While it's correct to maintain three separate `Holding` entities (one for each account), you want to create a single `Financial Instrument` entity for AMZN.

This resolved entity will link to multiple `Holding` entities, which themselves remain linked to their respective `Accounts`, maintaining account relationships.

To do this, click on "Add class to deduplicate entities" and select `Financial Instrument`.

Using natural language, simply state how you want duplicates to be identified. In this case, it's straightforward:

> "Merge instruments if they have the same symbol or CUSIP."

Click on "Generate rules" and you should see a set of conditions displayed on the right.

`Financial Instruments` that meet these conditions will be considered duplicates and auto-resolved. If there are conflicting properties (which shouldn't occur in this simple example), an LLM will resolve them based on best judgment, or based on merge guidelines you can also provide as "Advanced parameters".

Clicking "Deduplicate" performs the consolidation.

After deduplication, you will have 387 entities before deduplication and 290 entities after.

This step keeps account-specific holdings separate (for tax tracking or individual portfolio performance) while enabling portfolio-wide analysis, as we'll see in the next step. See for example "BKNG" `Financial Instrument` that is linked to 3 `Holdings` and originates from 3 source entities:

<details>
<summary>Advanced considerations: Sophisticated Deduplication</summary>
<br>
This step is also useful in more complex cases. For example, "Tesla" and "Tesla, Inc." can be reconciled using fuzzy matching on the corresponding property, plus AI review to make the final decision (a tutorial dedicated to complex entity resolution will come soon). 
</details>

Once complete, go to **Results** and create your **Knowledge Base**: this becomes your queryable, unified investment database ready for analysis!

## Step 4: Query Your Consolidated Portfolio

This is where it gets the most exciting! You can now run sophisticated queries across your unified portfolio data.

Navigate to **Ask** section. This will open a conversational interface. You should land on our dedicated "Knowledge Base Agent". If not, select it from the menu as in the screenshot below:



 - **Summary statistics**

Let's first verify that we can replicate summary statistics present in the account statements to confirm accuracy.

> Project ‘YOUR-PROJECT’, knowledge base ‘YOUR-KNOWLEDGE-BASE’, compute allocation (in terms of market value and % of total) by type of financial instrument, for each account. Take the cash position into account" 

You should have the exact same metrics that you can find in the accounts statements. That's a good start! Now we can perform other queries.

 - **Top holdings of the combined portfolios**

Perfect!

When provided with the three account statements, both the latest OpenAI and Anthropic models will fail at this query and other advanced queries (despite appearing confident in their responses), which outlines the strength of our approach. 

In fact, our approach is the only approach that scales well with large portfolios, different brokerage accounts using varying data structures and conventions, more sophisticated ontologies, and additional documents in the collection.

## Step 5 (Coming Soon): Create a Dashboard

**Blue Morpho** allows you to build dashboards to visualize your consolidated portfolio. Our dashboards are:
- **Interactive:** They feature filters, dropdown menus, and other interactive elements
- **Dynamic:** Adding documents to the collection (not yet supported) will iteratively grow the **Knowledge Base** and automatically populate the dashboards with fresh data

Simply describe what you want in natural language, and our system will generate the appropriate visualizations!

Here are the chart types you can use:
- Area Chart
- Area Stacked Chart  
- Bar Chart
- Bar Stacked Chart
- Line Chart
- Pie Chart
- Scatter Chart
