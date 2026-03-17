# CodeCrusher Architecture Notes

This document provides a high-level overview of the CodeCrusher system design.

The focus is on architecture and workflow thinking rather than internal implementation details.

---

## Design Goals

CodeCrusher was designed with the following goals in mind:

- reduce repetitive refactoring work
- keep developers in control of suggested changes
- support multiple LLM providers through routing and fallback
- make AI-assisted workflows observable and reviewable
- support repository-scale analysis rather than isolated snippets

---

## Main Components

### 1. CLI / Dashboard Layer

This is the entry point for developers.

It provides:

- repository scanning commands
- preview workflows
- log visibility
- user control before applying changes

---

### 2. Task Orchestrator

The orchestration layer coordinates work between repository analysis, model selection, and output processing.

Responsibilities include:

- dispatching tasks
- sequencing analysis steps
- handling retries and fallbacks
- coordinating preview vs apply workflows

---

### 3. Model Router

The model router determines which LLM provider should handle a task.

This layer exists because different tasks may perform better with different models depending on complexity, latency, and output quality.

Typical routing concerns include:

- provider availability
- task complexity
- fallback logic
- output confidence

---

### 4. LLM Provider Layer

This layer communicates with model providers.

Examples include:

- Mistral
- Mixtral
- LLaMA-based systems

The purpose of this layer is abstraction and flexibility rather than hard-coupling the tool to a single provider.

---

### 5. Diff / Validation Layer

One of the most important parts of the system is the validation step between model output and developer action.

This layer is responsible for:

- comparing suggested changes
- structuring diffs
- supporting preview workflows
- helping reduce unsafe or low-quality modifications

---

### 6. Output Layer

The final output is presented back to the developer in a form that can be reviewed before use.

This helps preserve developer control and improves trust in the workflow.

---

## Simplified Flow

```text
Developer
  ↓
CLI / Dashboard
  ↓
Task Orchestrator
  ↓
Model Router
  ↓
LLM Providers
  ↓
Diff / Validation Layer
  ↓
Output Suggestions
```

---

## Architecture Principles

A few principles shaped the design:

### Developer-in-the-loop
The system is intended to assist engineers, not replace review and judgment.

### Safe preview before apply
Suggested changes should be observable and reviewable before being accepted.

### Model flexibility
Different tasks may require different LLM providers, so routing matters.

### Observability
Logs, preview output, and workflow transparency are important in AI-assisted engineering systems.

---

## Note

This document describes the public project architecture at a high level and does not expose proprietary implementation details.
