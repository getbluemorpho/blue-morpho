# Platform Overview

The **Blue Morpho** platform is designed to help you build all kinds of **AI agents**, **apps**, and **workflows** on top of **enterprise data**.

## Why Blue Morpho?

The platform is built with a focus on making enterprise AI:

- **Scalable**: Our Knowledge Layer is a solid foundation to automate not just one workflow, but all your workflows.
- **Reliable**: The Knowledge Layer serves as well-structured context to AI agents, making it easy for them to find and use the right data.
- **Widely Accessible**: Blue Morpho brings together both technical builders and subject matter experts.

## Get Access

Ready to begin?  
[Sign up](https://app.getbluemorpho.com) to Blue Morpho and get access to the platform with a trial account.

## Introductory Concepts

### Collections

Raw input data is stored in **Collections**, which support two types of data:

- **Unstructured**: such as `.pdf`, `.txt`
- **Structured**: such as `.csv` (coming soon)

Collections typically originate from organizational data sources, either:

- Uploaded manually
- Or (coming soon) ingested programmatically via the **Blue Morpho SDK**

Connectors to enterprise sources can also be provided [upon request](mailto:hello@getbluemorpho.com).

### Extraction Runs

**Extraction Runs** extract, transform, and enrich the data from **Collections** to build the **Knowledge Graphs** following the format of the **Ontologies**.

### Ontologies

Ontologies define **business concepts** (e.g., `Driver`, `Vehicle`, `Trip`) and their **properties** (e.g., `name`, `address`, `SSN`).

### Knowledge Graphs

A Knowledge Graph connects **entities** with their **relationships**.  
For example, the Driver "John Doe" might be linked to his Vehicle and Trips using relations like `hasVehicle`, `hasTrip`.

### Knowledge Bases

Together, the Ontology and Knowledge Graph form a **Knowledge Base**.

Knowledge Bases ensure that your data is:

- **Well-defined**, thanks to the Ontology
- **Linked and resolved**, through relations in the Knowledge Graph

This structure makes it easy for AI agents to find and use the right data reliably.

### Internal Agents

Blue Morpho includes built-in agents for:

- Answering queries based on your data
- Building reports that auto-update as the Knowledge Base evolves

### External Agents

Agents that operate outside of Blue Morpho can connect to its Knowledge Layer using the **Model Context Protocol**.

---

## Next Steps

Functionally, the Blue Morpho platform delivers two core capabilities:

- **Building Knowledge Bases** from structured and unstructured enterprise data  
- **Operating AI agents** that reason and act on top of those Knowledge Bases
