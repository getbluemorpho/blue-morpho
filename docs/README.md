# Getting Started 

**Build AI apps on top of knowledge bases.**

Blue Morpho helps you build AI agents that understand your business context, using ontologies and knowledge graphs.

1. Connect your data  
2. Build knowledge bases in the form of ontologies and knowledge graphs.  
3. Expose your knowledge bases to internal (in Blue Morpho) or external (through MCP) agents.  
4. Use AI agents to create AI apps for the following use cases: chat with your data, build reports (static or updated when new data arrives), automate workflows.

➡️ [Sign up to Blue Morpho](https://app.getbluemorpho.com)

---

## Problem we are solving

Almost all enterprise workflows require gathering information from multiple sources + contextualizing it + applying business logic. Yet simply putting AI agents on top of enterprise systems doesn’t work:

❌ **Data is not indexed for agents to find it.** This makes it hard for them to locate relevant information. Vector embeddings are unreliable.

❌ **It is hard for agents to reconcile information from multiple sources.** If systems have ten different definitions of “customer”, or “Walmart” shows up in ten different forms, AI agents are doomed anyway.  

❌ **Agents lack contextual information about the data.** Data does not live in isolation. E.g. for procurement use cases, there is a need to link data across emails, contracts, invoices, approval logs, ERP logs, …  

❌ **Agents fail to capture business logic.** It’s in people’s heads, applications, documents, … It doesn’t belong there.  

---

## What makes us different

✅ **Reliable metadata filtering, instead of fuzzy vector search.**  
  We do not rely on vector embeddings for agents to locate relevant information. We leverage metadata filtering instead. Basically, LLMs “read” through all input documents and extract vast quantities of metadata derived from business concepts defined in an ontology. Metadata is then used by AI agents to retrieve the right context to perform their tasks, reliably.  

✅ **Entity resolution with LLM as a judge, instead of human review.**  
  We make it very simple to reconcile (“resolve”) information from multiple sources, making all information about a given entity accessible to AI agents from the same place.

✅ **Smart orchestration of LLM calls in controlled environments, instead of fully autonomous agents going wild.**  
  We have a focus on reliability and scalability of our approach. A concrete example for a dashboard building agent: instead of prompting LLMs to “code a dashboard for XYZ” from scratch, which is prone to many pitfalls making the approach unreliable ; we believe in a parametrized approach. LLMs are given standardized components (charts, buttons, menus) that they can parametrize (with cypher queries to fetch data based on input parameters, or with styling options), making the approach reliable and scalable.  

---

## Features

### Build knowledge bases (create an ontology + knowledge graph)

- Ingest and parse documents (pdf, txt)  
- Create ontologies (classes, properties and relations). Manage the different versions.  
- Process documents to extract a knowledge graph (entities and their relationships) following your ontology. Resolve (i.e. deduplicate) entities coming from different chunks/documents.  
- Turn your knowledge graph into a knowledge base, effectively exposing it to AI agents (internal or external).  

### Operate agents (to consume data in knowledge bases and act on it)

- Chat with your knowledge bases  
- Connect to your knowledge bases through Blue Morpho MCP server

---

## Coming soon

These features are under active development and will be released in the coming weeks:

- Build static or dynamic dashboards on top of your knowledge bases, using natural language  
- Merge two or more knowledge bases

---

## Feedback

We welcome feedback!  
Please open an [issue](https://github.com/getbluemorpho/blue-morpho/issues), or reach out to [hello@getbluemorpho.com](mailto:hello@getbluemorpho.com).

---

## Socials

[Website](https://www.getbluemorpho.com)

[LinkedIn](https://www.linkedin.com/company/get-blue-morpho/)

[GitHub](https://github.com/getbluemorpho/blue-morpho)

## Acknowledgements

This project uses the following open source projects:

- [Librechat](https://github.com/danny-avila/LibreChat): for chat interface  

We are grateful for their maintainers and contributors.
