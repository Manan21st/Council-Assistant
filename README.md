# CouncilGPT – Agentic Knowledge Management Assistant for Student Councils

Loom Link - https://www.loom.com/share/7d0705ce1f194850a71cc923ad21365a

## Problem Statement

Student councils frequently handle announcements, meeting notes, policy discussions, SOPs, hostel-related issues, event planning, and operational decisions. Over time, important information gets scattered across WhatsApp groups, emails, meeting records, and personal notes, making it difficult to retrieve past decisions and institutional knowledge.

CouncilGPT is an agentic AI workflow that acts as a centralized knowledge assistant for student councils. It stores important information, retrieves historical decisions using semantic search, performs external research when needed, and assists in generating operational content such as SOPs, emails, and announcements.

---

## Workflow Goal

The goal of the system is to:

- Store important council information as long-term memory.
- Retrieve previously stored information using semantic search.
- Answer council-related questions accurately.
- Generate SOPs, emails, and operational content.
- Perform external research when information is not available in memory.
- Demonstrate agentic workflow design using AI reasoning, tool usage, memory, and deterministic routing.

---

## System Architecture

```text
User Message
      │
      ▼
Intent Classifier (Gemini)
      │
      ▼
Deterministic Switch
 ┌─────────────┴─────────────┐
 │                           │
 ▼                           ▼
STORE PATH              AGENT PATH
 │                           │
 ▼                           ▼
Qdrant Memory          CouncilGPT Agent
Storage                     │
                            ├── SearchMemory Tool
                            │
                            └── Web Research Tool
```

---

## Technologies Used

### Workflow Platform
- n8n (v2.22.6)

### LLMs
- Google Gemini 2.5 Flash
- Google Gemini 3.0 Flash (upgrade path)

### Embeddings
- Google Gemini Embeddings

### Vector Database
- Qdrant

### External Research
- UpRock Web Crawler

### AI Capabilities
- Intent Classification
- Retrieval-Augmented Generation (RAG)
- Tool Calling
- Long-Term Memory
- Semantic Search
- SOP Generation
- Email Drafting
- Operational Assistance

---

## Workflow Breakdown

### 1. User Input

The workflow begins when a user sends a chat message.

Examples:

```text
Management agreed to review hostel fees after receiving student feedback.
```

```text
What did management agree regarding hostel fees?
```

---

### 2. Intent Classification Agent

A Gemini-powered classifier determines whether the message should be stored as knowledge or processed as a task/query.

Possible outputs:

```json
{
  "intent": "STORE"
}
```

or

```json
{
  "intent": "OTHER"
}
```

---

### 3. Deterministic Routing

A Switch node routes the workflow based on the classifier output.

#### STORE Route

Information is stored in long-term memory.

#### OTHER Route

The query is forwarded to the CouncilGPT Agent.

This separation ensures that AI handles reasoning while deterministic logic controls execution.

---

### 4. Memory Storage Pipeline

For messages classified as STORE:

1. Content is extracted.
2. Gemini Embeddings generates vector representations.
3. Vectors are stored in Qdrant.
4. Information becomes searchable in future conversations.

Example:

```text
Management agreed to review hostel fees after receiving student feedback.
```

Stored as semantic memory.

---

### 5. CouncilGPT Agent

For non-storage requests, the AI Agent receives the user query.

The agent can:

- Answer questions
- Draft SOPs
- Generate announcements
- Draft emails
- Search memory
- Perform external research

---

### 6. Memory Retrieval (RAG)

The agent uses the SearchMemory tool when answering council-related factual questions.

Example:

Input:

```text
What did management agree regarding hostel fees?
```

Retrieved Memory:

```text
Management agreed to review hostel fees after receiving student feedback.
```

Response:

```text
Management agreed to review hostel fees after receiving student feedback.
```

This enables Retrieval-Augmented Generation (RAG).

---

### 7. External Research

When information is not present in memory, the agent can use the UpRock Web Research tool.

Example:

```text
What are the latest UGC hostel guidelines?
```

The agent retrieves external information and uses it to generate a response.

---

## Agentic Practices Demonstrated

### Role-Based AI

#### Intent Classifier

Responsible for determining workflow routing.

#### CouncilGPT Agent

Responsible for reasoning, retrieval, drafting, and research.

---

### Tool Usage

The agent has access to:

- SearchMemory
- UpRock Research Tool

The agent autonomously decides when to use them.

---

### Long-Term Memory

The workflow stores information inside Qdrant using vector embeddings, allowing future retrieval through semantic search.

---

### Retrieval-Augmented Generation

The agent combines:

- User Query
- Retrieved Memory
- LLM Reasoning

to produce context-aware answers.

---

### Deterministic Control

The workflow uses:

- Merge Nodes
- Switch Nodes
- Filters
- Storage Pipelines

for predictable execution.

---

## AI vs Deterministic Logic

### AI Components

- Intent Classification
- Question Answering
- SOP Generation
- Email Drafting
- Semantic Retrieval
- Research Interpretation

### Deterministic Components

- Workflow Routing
- Memory Storage
- Embedding Generation
- Vector Search Execution
- Filtering
- Branch Control

This separation follows agentic workflow design principles.

---

## Example Use Cases

### Knowledge Storage

```text
Store this:
Hostel committee meeting scheduled on Friday at 7 PM.
```

Result:

Stored in Qdrant memory.

---

### Knowledge Retrieval

```text
When is the hostel committee meeting?
```

Result:

```text
The hostel committee meeting is scheduled on Friday at 7 PM.
```

---

### SOP Generation

```text
Create a short SOP for meeting guidelines.
```

Result:

Generated SOP document.

---

### Email Drafting

```text
Draft an email requesting hostel maintenance.
```

Result:

Professional email draft.

---

### Research

```text
What are the latest hostel regulations?
```

Result:

Agent performs web research and summarizes findings.

---

## Current Limitations

The current implementation focuses on demonstrating agentic workflows, memory, retrieval, and tool usage.

### Document Ingestion

Not currently supported:

- PDF uploads
- PDF URLs
- Google Docs URLs
- Google Sheets URLs
- DOCX files
- Spreadsheet uploads

Users must currently provide information as text.

---

### Automatic Extraction

The system does not yet:

- Extract text from PDFs
- Parse Google Docs automatically
- Parse Google Sheets automatically
- Crawl and store entire documents

---

### Notion Integration

Notion integration is not currently connected.

Although the agent can generate SOPs, announcements, and documents, it does not automatically create Notion pages.

A Notion MCP integration was explored but was excluded due to compatibility issues with Gemini tool-calling schemas.

---

### Single-Agent Architecture

The workflow currently uses:

- Intent Classifier
- CouncilGPT Agent

Future versions can introduce dedicated agents such as:

- Research Agent
- Documentation Agent
- Approval Agent

---

## Future Improvements

### Knowledge Ingestion

- PDF Upload Support
- PDF URL Ingestion
- Google Docs Ingestion
- Google Sheets Ingestion
- Bulk Document Import

### Documentation

- Google Docs Creation
- Notion Page Creation
- Automatic SOP Publishing
- Meeting Minutes Generation

### Advanced Agentic Features

- Multi-Agent Architecture
- Dedicated Research Agent
- Dedicated Documentation Agent
- Human Approval Workflows
- Confidence-Based Routing
- Validation Agents

### Council Operations

- Event Planning Assistant
- Action Item Tracking
- Policy Change Tracking
- Decision Logging
- Automated Announcements

---

## Sample Workflow Demonstration

### Store Information

Input:

```text
Management agreed to review hostel fees after receiving student feedback.
```

Classification:

```text
STORE
```

Action:

```text
Stored in Qdrant memory.
```

---

### Retrieve Information

Input:

```text
What did management agree regarding hostel fees?
```

Action:

```text
SearchMemory Tool Invoked
```

Output:

```text
Management agreed to review hostel fees after receiving student feedback.
```

---

## Learning Outcomes

This project demonstrates:

- Problem-to-workflow mapping
- Agentic workflow design
- Retrieval-Augmented Generation (RAG)
- Tool usage
- Long-term memory systems
- AI reasoning with deterministic control
- Practical application of n8n for real-world automation

---

## Author

**Manan Agrawal** (23bcs10206)

Built as part of the **Agentic Workflow Design and n8n Demo Assignment**.
