# **Analyze Pitch Decks**

VC analysts review dozens of pitch decks every week, often repeating the same tasks: extracting the core details of a startup, checking whether it fits the fund’s investment thesis, identifying risks, comparing it to similar companies, and preparing insights for partners. In this tutorial, you’ll learn how to automate much of that workflow using Blue Morpho.

We’ll work through a set of sample pitch decks and show how to transform raw PDFs into structured insights, apply deal‑fit logic, detect risks, and query everything through a knowledge base. 

The focus is on pitch deck analysis, but the same approach applies to many other workflows, including investment memo analysis, dealflow reporting, and startup benchmarking.

## **Step 1: Create a Project**

A project keeps your collections, ontology, extraction runs, and knowledge bases organized in one place.

1. Go to **Projects**.  
2. Click **New Project**.  
3. Add a name (e.g., *“Pitch Deck Analysis”*).  
4. Add a description (e.g., *“A project to analyze startup pitch decks.”*).  
5. Click **Create**.

## **Step 2: Create a Collection**

A Collection is a folder of documents processed with the same ontology. Upload your pitch decks here so Blue Morpho can parse and analyze them. For this tutorial, the dataset contains several well-known public startup pitch decks from different stages and industries.

1. Go to **Collections**.  
2. Click **New Collection**.  
3. Add a name (e.g., *“Pitch Deck Dataset”*).  
4. Add a description (e.g., *“A set of startup pitch decks.”*).  
5. Upload the sample pitch decks. [Download here](https://github.com/getbluemorpho/blue-morpho/tree/643f6b6de88b6fabb8f307b7bf33aa64cfd52712/docs/assets/tutorial-analyze-pitch-decks).   
6. Click **Create**.

## **Step 3: Create an Ontology with the AI Assistant**

In this step, you will teach Blue Morpho how to understand pitch decks the way a VC analyst does. By the end, you’ll have an ontology that:

* Extracts structured information about each startup,  
* Applies your investment thesis to classify deal fit,  
* And can evolve over time through version updates.

### **Phase 1: Create the base ontology**

The ontology defines the core concepts you want Blue Morpho to extract from your pitch decks. Instead of building it manually, you can describe your needs in natural language and let the **AI Ontology Assistant** generate the first version.

1. Go to **Ontologies**.  
2. Click **New Ontology**.  
3. Add a name (e.g., *“Pitch Deck Ontology”*).  
4. Add a description (e.g., *“Structure for extracting insights from pitch decks.”*).  
5. Select **AI Ontology Assistant**.

When the assistant asks what the ontology should capture, describe the key elements you want extracted from each pitch deck. This helps the assistant generate the right structure for your use case.

Prompt to use:

```
I want to analyze startup pitch decks like a VC analyst.

I need the ontology to extract and structure the most important elements from any pitch deck so I can compare companies, identify red flags, and generate investment memos.

The ontology should capture:

* Company basics: name, industry, business model, geography, stage  
* Product & solution: problem, solution, demo screenshots, GTM, pricing  
* Market data: TAM, SAM, SOM, target customer, market trends  
* Traction & metrics: revenue, MRR, growth rate, churn, CAC, LTV, users, retention  
* Team: founders, roles, background, previous exits  
* Competition: competitors, differentiation, moat  
* Financials: round type, funding ask, use of funds
```

#### **Review your generated ontology**

Once generated, review the proposed classes and properties and check whether they match your workflow:

* Are the classes, properties and relations clear?   
* Should some classes be merged or simplified?  
* Are any important properties missing?

You can refine your ontology at any time. Tell the AI assistant what you want to adjust, and it will save those changes as a **new version** so you can track how your ontology evolves.

### **Phase 2: Enrich your ontology with business context**

After creating the base structure, you can teach the ontology how you actually evaluate startups. This is where domain knowledge becomes part of the system.

**1\. Identify only the primary company**

Pitch decks mention many companies—competitors, customers, partners. Only the startup being pitched should be extracted as a `Company`. Everything else should map to other concepts. We can ask the assistant to create this rule. 

Prompt to use:

```
Ensure the Company class is populated only with the primary startup of the deck and not with any other companies mentioned.
```

**2\. Add deal-fit logic based on your investment thesis**

To help Blue Morpho evaluate whether each startup aligns with your fund’s investment criteria, you can add business rules that assess deal-fit directly from the extracted information. This allows the system to classify opportunities automatically and explain the reasoning in natural language.

Prompt to use:
```
I want the ontology to automatically judge whether a startup matches our investment preferences. Please add a ‘investment fit’ concept that the system can fill in using rules based on what we look for, and have it explain the reasoning in plain language.

What we look for:

* Early B2B SaaS companies  
* Seed to Series A  
* Some early traction  
* Strong technical founders  
* Raising between €300k and €2M.
```

**3\. Track risks inferred from extracted data**

Finally, you can extend your ontology to detect risks automatically based on the information extracted from each pitch deck. This turns risk analysis into an automated layer that highlights missing data, weak signals, or unrealistic claims.

Instead of manually identifying potential issues, you can let Blue Morpho infer them by creating a `Risk` concept and defining simple rules—such as flagging missing traction, unclear business models, unrealistic market sizes, or weak founder backgrounds.

Prompt to use:
```
Use the Risk concept to automatically flag potential issues (e.g., missing traction, unclear business model, unrealistic TAM, weak team signals) and set a risk type and severity based on what you extract from the pitch deck.
```

## **Step 4: Run an Extraction**

Now it’s time to extract the key insights from each pitch deck using your ontology.

### **Steps**

1. Go to **Extraction Runs**.  
2. Click **New Run**.  
3. Add a name (e.g., *“Pitch Deck Analysis Extraction”*).  
4. Add a description (e.g., *“Extract structured insights from the pitch decks.”*).  
5. Select your Ontology and your Collection.  
6. Click **Create**.  
7. Review the default settings and click **Extract**.  
8. Skip Deduplication (not needed for pitch decks).  
9. Open **Results**.

### **Validate results**

Use the filters at the top of the Results view to select **Airbnb-pitch-deck.pdf** under *Documents* and **Company** under *Classes*. This will show the extracted Company entity along with all its related information.

Check that the core details look correct (industry, business model, geography, stage) and that the linked Product, Market, Traction, or Funding entities reflect what’s in the deck. Make sure the Investment-Fit assessment makes sense based on your thesis, and review any risks the system inferred.

If something looks off, go back to the AI Ontology Assistant, refine your instructions, and rerun the extraction. A couple of quick iterations is normal until the results look right.

## **Step 5: Create a Knowledge Base**

Once you’re happy with the extraction results, the next step is to make them queryable.

1. From the Extraction Results view, click **Create Knowledge Base**.  
2. Add a name (e.g., *“Pitch Deck Analysis KB”*).  
3. Add a description (e.g., *“A knowledge base with all extracted pitch deck insights.”*).  
4. Click **Create**.

## **Step 6: Ask Questions**

With your Knowledge Base ready, you can now interact with all your pitch decks through natural language.

1. Go to **Knowledge Bases**.  
2. Open your *Pitch Deck Analysis KB*.  
3. Click **Ask** to start a chat.

### **Example 1 – Overview of all startups**

Get a quick, structured snapshot of every company in your dataset.
```
Give me an overview of all startups with their company details in a clean table
```
<img width="827" height="187" alt="ex1" src="https://github.com/user-attachments/assets/c435d693-22b2-41b2-b6c3-d23ef0a9205c" />

### **Example 2 – Analyze products and value propositions**

See how each startup positions its product and the value it delivers to customers.
```
Analyze the product and value proposition for Airbnb
```
<img width="813" height="287" alt="ex2" src="https://github.com/user-attachments/assets/65d43dbc-0426-4d18-8e70-2b425809ff63" />

### **Example 3 – Evaluate investment‑fit**

Use the business logic you added earlier to understand which startups match your thesis.
```
How startups fit our investment thesis, and why? Output in a clean table view
```
<img width="824" height="224" alt="ex3" src="https://github.com/user-attachments/assets/17af5227-9af6-475f-a200-8b513a4ca2e4" />

### **Example 4 – Identify risks**

Surface red flags automatically inferred from each pitch deck.
```
What are the top risks identified for Airbnb?
```
<img width="815" height="266" alt="ex4" src="https://github.com/user-attachments/assets/758b9a21-0847-4a63-be9d-aeaf0024c6d5" />

### **Example 5 – Generate a partner‑ready summary**

Produce a clean brief for partner meetings or investment committees.
```
Create a partner‑ready summary about each startup
```
<img width="868" height="646" alt="ex5 1" src="https://github.com/user-attachments/assets/7e6248da-50c4-42ac-ac3a-22fdd94ab342" />

### **Bonus queries**

* Which companies have strong technical founders?  
* Show all startups with missing traction or incomplete data.  
* Which companies have unrealistic market claims?  
* Find all companies raising more than €2M.

Use follow‑up questions to drill deeper, just like you would in a real investment discussion.

## **Step 7: Next steps**

You now have a complete workflow for analyzing pitch decks with Blue Morpho: upload decks, model your investment logic as an ontology, extract structured data, and query everything through a Knowledge Base.

Here are some ideas for what to try next:

* Upload your own pitch decks and refine the ontology to match your fund’s thesis.  
* Extend the ontology with additional concepts, such as unit economics or references to market benchmarks.  
* Explore how this same approach can be applied to other workflows such as [ETF analysis](https://docs.getbluemorpho.com/tutorials/analyze-etfs-with-blue-morpho/), [NDA compliance](https://docs.getbluemorpho.com/tutorials/automate%20nda%20review/) reviews, or consolidating [investment portfolios](https://docs.getbluemorpho.com/tutorials/consolidate%20investment%20portfolios/).
