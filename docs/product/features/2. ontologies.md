# Ontologies

## Overview

An Ontology in Blue Morpho is a domain model that defines the types of entities, their properties, and relationships that can be extracted from your documents. Think of it as a blueprint that tells the system what to look for in your data and how to organize the knowledge.

**The ontology represents your ideal data model.** Information from your documents will be extracted, transformed, enriched and mapped to fit this structured model. The goal is not to stick exactly to how information appears in your documents, but to reorganize it into a consistent format that serves your business needs.

## Core Concepts

### What is an Ontology?

An Ontology is a formal specification that:
- Defines **Classes** (like Person, Organization, Event) present in your data
- Specifies **Properties** for each entity type (like name, date, location)
- Establishes **Relationships** between different entity types (like a Person attending an Event)
- Serves as a schema for knowledge extraction and knowledge graph building, ensuring consistent data structure

### Why Use Ontologies?

Ontologies enable:
- **Structured Knowledge Extraction**: Extract specific information types from your documents, consistently
- **Powerful Querying**: The ontology is not only used for extraction, it is also passed as context for AI agents to query your data.

## Ontology Components

### Classes

Classes define the categories of "things" that can be extracted from documents.

**Examples:**
- `Person` - Individual people mentioned in documents
- `Organization` - Companies, institutions, groups
- `Location` - Places, addresses, geographical areas
- `Event` - Meetings, conferences, incidents

An instance of a class is an entity. For example, "Abraham Lincoln" is an entity belonging to the class `Person`. 

### Properties

Properties define the attributes that entities can have. Each property has:

- **Name**: The property identifier (e.g., "name", "date_of_birth")
- **Description**: What this property represents
- **Data Type**: The type of data this property stores
- **Required Status**: Whether this property must have a value
- **Possible Values**: For enumerated properties, the allowed values

**Supported Data Types:**
- `str` - Text values (e.g., names, descriptions)
- `int` - Whole numbers (e.g., age, count)
- `float` - Decimal numbers (e.g., price, percentage)
- `bool` - True/false values
- `date` - Calendar dates
- `datetime` - Date and time combinations
- `time` - Time values
- `duration` - Time periods

**Enumerated Properties:**
For `str` and `int` properties, you can specify allowed values using the "enum" function. 

### Relationships

Relationships define how different entity types connect to each other.

**Structure:**
- **Source Type**: The entity type that initiates the relationship
- **Target Type**: The entity type that receives the relationship
- **Predicate**: The relationship description

**Examples:**
- `Person` → `works_for` → `Organization`
- `Person` → `lives_in` → `Location`
- `Organization` → `located_in` → `Location`
- `Event` → `attended_by` → `Person`

## Ontology Families and Versions

### Ontology Families

An **Ontology Family** is a group of related ontology versions that evolve over time. This allows you to iterate on your ontology design while tracking your changes and maintaining backwards compatibility. 

### Creating Ontology Versions

You can create new ontology versions in two ways:

#### From Scratch
Start with a completely new ontology design using our ontology editor, or upload a YAML file with the following structure:

```yaml
entity_types:
- name: Account
  description: Brokerage account containing financial holdings
  properties:
  - name: accountNumber
    description: unique account identifier
    type: str
    is_required: false
    possible_values: null
  - name: accountType
    description: type of account (e.g., Individual, IRA)
    type: str
    is_required: true
    possible_values: null
  - name: baseCurrency
    description: base currency using ISO codes (USD, EUR, etc.)
    type: str
    is_required: true
    possible_values: null

- name: FinancialInstrument
  description: Any financial instrument available for investment
  properties:
  - name: symbol
    description: ticker symbol or unique identifier
    type: str
    is_required: true
    possible_values: null
  - name: instrumentType
    description: type of instrument
    type: str
    is_required: true
    possible_values:
    - stock
    - bond
    - etf
    - mutual fund

relation_types:
- subject_name: Account
  predicate: Contains
  object_name: Holding
- subject_name: Holding
  predicate: Has
  object_name: FinancialInstrument
```

#### From Modifications
Build upon an existing ontology version with incremental changes by simply editing and saving an existing version.

## Best Practices

### Ontology Design

- Start simple: Begin with core classes and properties your domain needs
- Test ontology changes with sample documents and iterate based on extraction quality
- Use clear, descriptive names and descriptions for types and properties, as these fields are used by AI at extraction and querying times. 
