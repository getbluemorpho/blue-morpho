# Analyze Investment Portfolios 

If you have different investment account types (IRAs, 401(k)s, taxable accounts) or use different brokers, you know how tedious it is to get a complete picture of your holdings. This tutorial shows you how to use Blue Morpho to:

- Consolidate identical holdings across multiple portfolios for a unified view
- Perform aggregations, calculations, and analysis
- Create interactive dashboards to manage your holdings

This use case demonstrates the strength of our approach, as these results cannot be obtained using any state-of-the-art model out of the box.

 **Similar Use Cases**

This approach works for any use case where you need to structure and homogenize data for structured queries (essentially, "turn data into tables and make it queryable"). Similar examples include:

- **Ledger consolidation:** Extract journal entries, accounts, and amounts from PDF exports to unify multiple accounting systems
- **Portfolio management for Private Equity:** Create portfolio management applications and automate reporting to LPs
- **Asset management & maintenance:** Merge asset registries from different plants or fleets to enable fleet-wide analytics
- **Healthcare records analysis:** Structure patient information, diagnoses, procedures, and billing codes from PDF medical records for analysis

## Step 1: Upload Your Portfolio Statements

Go to the **Collections** section and create a new collection containing your investment statements (PDFs). For this tutorial, we'll use sample statements from Interactive Brokers. The statements cover three different brokerage accounts:

- A traditional IRA
- A Roth IRA  
- A regular taxable account

You can download them [here](https://github.com/getbluemorpho/blue-morpho/tree/main/docs/assets/Interactive%20Brokers%20sample%20statements).

Upon upload, **Blue Morpho** will automatically parse these documents, transforming them into raw text while preserving table structures to feed them to LLMs in the next steps.

## Step 2: Define Your Ontology

When consolidating multiple portfolios, key concepts (ontology classes) include `Account`, `Cash Balance`, `Holding`, and `Financial Instrument`.

<img width="683" height="341" alt="ontology-diagram" src="https://github.com/user-attachments/assets/02e7356f-0e06-4632-a398-61d00351ef3d" />

Navigate to the **Ontologies** section and create a new ontology. You can name it "consolidate portfolios". After the creation, click on "New version": you can either use the editor or download our [example ontology in YAML format](https://github.com/getbluemorpho/blue-morpho/blob/main/docs/assets/consolidate%20portfolios%20ontology.yaml) and import it directly into **Blue Morpho**.

You should see the following classes: 

<img width="615" height="530" alt="image" src="https://github.com/user-attachments/assets/c9ba3341-0562-43da-aa29-475cceefa340" />

<img width="614" height="317" alt="image" src="https://github.com/user-attachments/assets/244f5886-28a1-4e9b-b515-14389d4fb910" />

<img width="617" height="419" alt="image" src="https://github.com/user-attachments/assets/baa1b315-cdac-4e51-aee2-2e4669259300" />

<img width="615" height="466" alt="image" src="https://github.com/user-attachments/assets/705e98c5-5a57-4a61-b2e9-69bc79b8ea6a" />

And the following relationships:

<img width="639" height="501" alt="image" src="https://github.com/user-attachments/assets/61bc5e6d-ed79-4232-b9b4-e30943b5efa9" />

If desired, you can refine the ontology further by modeling `Transactions`, `Dividends`, and other financial concepts.

## Step 3: Run an Extraction

Navigate to the **Extraction Runs** section and start a new run using your ontology and document collection.

### 3.1 Chunking
This step is not needed for portfolio statements (it's reserved for very long documents). Navigate directly to **Extraction**. 

### 3.2 Extraction
The default settings work well and there is no need to configure advanced parameters. Click "Extract" to perform the extraction.

At this stage, you should have extracted 3 `Accounts`, 192 `Financial Instruments`, 184 `Holdings` and 7 `Cash Balances`.

As you can see in the results section, relations have also been extracted. For example, this particular `Holding` is linked with one `Financial Instrument` and one `Account`:

<img width="1212" height="642" alt="image" src="https://github.com/user-attachments/assets/3b56bd45-a8b6-406a-9211-809d46fb221e" />

??? note "Advanced considerations: How Blue Morpho Handles Different Broker Data Formats"
    Different brokers format data inconsistently:

    - Some classify interest payments or dividends as transactions, others don't
    - Some show sell transaction amounts as negative quantities, others as positive
    - Date formats, currency symbols, and field names vary across platforms

    **What you can do:**

    - **Use descriptive class definitions**: For example, if you only want stock/bond transactions (excluding interest payments or dividends), state this in your `Transaction` class description
    
    <img width="612" height="94" alt="image" src="https://github.com/user-attachments/assets/d2997e72-c4d1-49f3-be55-2a4cf5bcb34a" />

    - **Use descriptive property definitions**: For example, if you want sell transaction amounts as positive quantities, specify that all `quantity` and `amount` values should be positive numbers and add a `transaction_type` property that is either "sell" or "buy".
    
    <img width="608" height="528" alt="image" src="https://github.com/user-attachments/assets/b3c4849e-5dcc-4692-9748-42914ffa56b8" />

    Blue Morpho will automatically transform inconsistent broker data into clean, standardized entities that follow your exact specifications. 

### 3.3 Deduplication
This critical step consolidates duplicate entities across your multiple accounts. For example, Amazon (AMZN) stock is held in all three accounts, so it was extracted three times as separate `Financial Instrument` entities. While it's correct to maintain three separate `Holding` entities (one for each account), you want to create a single `Financial Instrument` entity for AMZN.

This resolved entity will link to multiple `Holding` entities, which themselves remain linked to their respective `Accounts`, maintaining account relationships.

To do this, click on "Add class to deduplicate entities" and select `Financial Instrument`.

Using natural language, simply state how you want duplicates to be identified. In this case, it's straightforward: "Merge instruments if they have the same symbol or CUSIP."

<img width="1270" height="640" alt="image" src="https://github.com/user-attachments/assets/7159f869-95a1-420d-928b-47b7a1a412d1" />

Click on "Generate rules" and you should see a set of conditions displayed on the right.

`Financial Instruments` that meet these conditions will be considered duplicates and auto-resolved. If there are conflicting properties (which shouldn't occur in this simple example), an LLM will resolve them based on best judgment, or based on merge guidelines you can also provide as "Advanced parameters".

Clicking "Deduplicate" performs the consolidation.

After deduplication, you will have 386 entities before deduplication and 289 entities after.

This step keeps account-specific holdings separate (for tax tracking or individual portfolio performance) while enabling portfolio-wide analysis, as we'll see in the next step. See for example "BKNG" `Financial Instrument` that is linked to 3 `Holdings` and originates from 3 source entities:
<img width="1224" height="282" alt="image" src="https://github.com/user-attachments/assets/3d16fb82-299e-462e-b105-552e02e98253" />


??? note "Advanced considerations: Sophisticated Deduplication"
    This step is also useful in more complex cases. For example, "Tesla" and "Tesla, Inc." can be reconciled using fuzzy matching on the corresponding property, plus AI review to make the final decision (a tutorial dedicated to complex entity resolution will come soon). 

    Once complete, go to **Results** and create your **Knowledge Base**: this becomes your queryable, unified investment database ready for analysis!

## Step 4: Query Your Consolidated Portfolio

This is where it gets the most exciting! You can now run sophisticated queries across your unified portfolio data.

Navigate to **Ask** section. This will open a conversational interface. You should land on our dedicated "Knowledge Base Agent". If not, select it from the menu as in the screenshot below:

<img width="621" height="175" alt="image" src="https://github.com/user-attachments/assets/71a666b8-aa08-4ec2-8c24-a46b5cb7a75f" />

### Select your Knowledge Base

> "Project ‘YOUR-PROJECT’, knowledge base ‘YOUR-KNOWLEDGE-BASE’

### Query: Summary statistics
 
Let's first verify that we can replicate summary statistics present in the account statements to confirm accuracy: 

> Compute allocation (in terms of market value and % of total) by type of financial instrument, for each account. Take the cash position into account.

The agent should return:

<img width="833" height="632" alt="image" src="https://github.com/user-attachments/assets/38a63385-da9c-437c-a5a0-5dddbff6b128" />

You should have the exact same metrics that you can find in the accounts statements. For example, in the traditional IRA account statement, we can find:

<img width="390" height="157" alt="image" src="https://github.com/user-attachments/assets/20a26d14-97e6-4cba-9d6b-cd1df6e9b436" />

The numbers match (although the aggregated information is of course not in the knowledge base), that's a good start! Now we can perform other queries.

### Query: Top holdings of the combined portfolios

Now let’s aggregate across accounts.

> What are the top 10 financial instruments by holding market value of the consolidated portfolio? For each instrument, indicate in how many accounts it can be found.

<img width="836" height="627" alt="image" src="https://github.com/user-attachments/assets/ff768c82-5451-427a-8853-5485e8d94ba7" />

Perfect!

When provided with the three account statements, both the latest OpenAI and Anthropic models will fail at this query and other advanced queries (despite appearing confident in their responses), which outlines the strength of our approach. 

In fact, our approach is the only approach that scales well with large portfolios, different brokerage accounts using varying data structures and conventions, more sophisticated ontologies, and additional documents in the collection.

## Step 5: Create a Dashboard (Coming Soon)

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
