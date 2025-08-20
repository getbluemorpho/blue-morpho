# What are Collections?

Collections are folders that organize your documents within a project in a logical way. Group documents in a collection based on whether you want to extract the same types of information from all of them (as one ontology will apply to all of them).

## Supported Types

**Unstructured:**
- PDF (automatically parsed into markdown format after upload)
- TXT

**Structured:**
- CSV (coming soon)

For high volumes of data (requiring programmatic ingestion) or currently unsupported data types, we can ingest any data for you upon request: [hello@getbluemorpho.com](mailto:hello@getbluemorpho.com).

## Parsing

After upload, your PDF documents are automatically parsed into text (more precisely, markdown) format. Title hierarchy and tables (even complex tables with subcolumns, etc.) are properly extracted but you might lose information from charts and pictures.

**Coming soon:** ability to skip parsing and treat raw PDFs. Parsing will still be the recommended option for documents containing mostly text and tables, as it makes further processing more efficient.

## Best Practices

- Use clear, descriptive names and descriptions for collections, as they are passed to AI agents
- Group documents in collections based on the types of information you want to extract from them. For example, group financial reports (extract financial metrics, management guidance, ...) or legal contracts (extract clauses, parties, ...) together; but don't mix one financial report and one contract together even if they are linked to the same company. Don't worry, you will be able to make that link later on.
