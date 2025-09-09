# Automate NDA Compliance 

Does your legal team review endless NDAs, always checking for the same compliance criteria?  
This tutorial shows you how to automate that review process using **Blue Morpho**.

**Similar use cases** 

This approach works for any use case where you need to validate a set of documents against a defined set of rules. Similar examples include:

- Ensuring contracts meet GDPR compliance  
- Screening RFPs for eligibility requirements  
- In M&A, parsing data room documents to extract relevant diligence signals

---

## Step 1: Upload Your NDAs

Go to the **Collections** section and create a new collection containing your NDAs (PDFs or text files).

Blue Morpho will automatically convert these documents to plain text if needed ("parse" them). This prepares them for further processing by LLMs. 
This may take a few minutes depending on the number and size of the files.

## Step 2: Define Your Ontology

To check NDAs against internal compliance rules, you’ll need to define key concepts using an **Ontology**.  
For example, you might want to check the following points:

- Is the NDA mutual, allowing both parties to share confidential information?
- Does the NDA preserve IP ownership for the disclosing party?
- Is the jurisdiction listed among your approved legal regions?

These criteria become **classes** in your ontology—each one representing a rule you want to validate.

### Example: `MutualityProvision` Class

<img width="653" height="313" alt="image" src="https://github.com/user-attachments/assets/0267bc13-358a-4f30-9e10-ed879cdf7424" />

Mutuality is often scattered across multiple clauses, it’s not enough to just check if the title says “Mutual NDA.” That’s why the `supporting_elements` property is important: it captures the specific passages in the NDA that justify the compliance decision.

You can create your own ontology from scratch, or download our [example NDA review ontology](https://github.com/getbluemorpho/blue-morpho/blob/main/docs/assets/review%20nda%20ontology.yaml) and import it in the app using the “Import from YAML file” function.

## Step 3: Run an Extraction

Navigate to the **Extraction runs** section and start a new run using your ontology and document collection.

- **Chunking**: NDAs are usually short, so you can skip this step unless your documents are unusually long.
- **Extraction**: Add an extraction step. The default settings typically work well. Click **“Extract”** to perform the extraction and review the results.
- **Resolution**: This step is optional and not needed for most NDA review workflows. You can skip it.
- Once the extraction is complete, go to **Results** and create your **Knowledge Base** to make the output accessible to agents.

Your Knowledge Base should now be visible in the **Knowledge Base** section.

## Step 4: Connect Your Knowledge Base to Cursor

[Cursor](https://cursor.com) is an AI-powered document editor that lets you review, accept, or reject suggested changes directly in the interface. While primarily used for code, it's excellent for any document editing.

### Setup

1. Install Cursor and connect Blue Morpho following [these steps](../product/setup-blue-morpho-mcp.md).
2. Open your NDA folder: **File > Open Folder**
3. Create a new chat (top right corner)
4. Select your target NDA

### Example Prompts

- **Find issues**

> "Using Blue Morpho, project ‘YOUR-PROJECT’, knowledge base ‘YOUR-KNOWLEDGE-BASE’, find non-compliant clauses for this NDA."

In my case, Cursor finds the only breach is the following:

<img width="519" height="125" alt="image" src="https://github.com/user-attachments/assets/0a7f3683-2332-4ddf-816c-7b7ff9c572d3" />

- **Fix them:**

> "Edit the NDA to make it compliant."

Cursor lets me review suggested changes in a nice interface where I can accept or reject them:

<img width="1337" height="116" alt="image" src="https://github.com/user-attachments/assets/79697a0e-85f7-4d06-8d14-ec4afe1f04ec" />

- **Update Blue Morpho**

> "Update the compliance status in Blue Morpho (using write queries)."

This keeps your Knowledge Base in sync with the latest version.  

<img width="421" height="169" alt="image" src="https://github.com/user-attachments/assets/bc3069da-c245-42a6-b781-c6733aeaaa36" />


**Note:** The Knowledge Base has been successfully updated by your document edits will be saved to your local folder only, not to Blue Morpho. We're working on automatic sync with Blue Morpho collections: stay tuned!
