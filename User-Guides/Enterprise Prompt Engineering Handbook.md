# Enterprise Prompt Engineering Handbook
## Practical instructions for writing effective AI instructions that get consistent, accurate results and avoid common pitfalls.

**Version**: 1.0  
**Author**: Corey Rollins  
**Date**: May 20, 2025

---

## Table of Contents

- [1. Introduction](#1-introduction)
- [2. Prompt Design Fundamentals](#2-prompt-design-fundamentals)
- [3. Common Pitfalls and How to Avoid Them](#3-common-pitfalls-and-how-to-avoid-them)
- [4. System Prompt Strategies](#4-system-prompt-strategies)
- [5. Role-Based Prompting Patterns](#5-role-based-prompting-patterns)
- [6. Multi-Turn Interactions](#6-multi-turn-interactions)
- [7. Evaluation and Refinement Techniques](#7-evaluation-and-refinement-techniques)
- [8. Use Case Templates](#8-use-case-templates)
- [9. Governance and Risk Considerations](#9-governance-and-risk-considerations)
- [Appendix A: Prompt Debugging Checklist](#appendix-a-prompt-debugging-checklist)
- [Appendix B: Reference Links](#appendix-b-reference-links)

---

## 1. Introduction

Prompt engineering is the discipline of designing effective instructions for large language models (LLMs). In an enterprise context, good prompts help reduce hallucinations, increase consistency, and align model behavior with business needs.

This guide provides practical frameworks for writing, evaluating, and managing prompts across departments, with examples and templates you can reuse.

---

## 2. Prompt Design Fundamentals

A well-engineered prompt includes:

| Component       | Description                                           | Example                                                   |
|----------------|-------------------------------------------------------|-----------------------------------------------------------|
| **Instruction** | What the model should do                             | “Summarize this text in 3 bullet points.”                 |
| **Input**       | The source content (text, data, etc.)                | “Customer chat log, JSON payload, markdown document”      |
| **Constraints** | Limits or boundaries                                 | “Use no more than 150 words. Exclude pricing details.”    |
| **Format**      | Expected structure of the output                     | “Respond in markdown table format with 3 columns.”        |
| **Tone/Role**   | The model’s persona or perspective                   | “You are a professional cybersecurity analyst.”           |

> **Tip:** Start simple. Test incrementally with clear constraints before adding complexity.

---

## 3. Common Pitfalls and How to Avoid Them

| Pitfall                    | Cause                                | Fix                                                          |
|---------------------------|--------------------------------------|--------------------------------------------------------------|
| Vague Instructions         | Ambiguous verbs or goals             | Use direct, imperative phrasing                              |
| Inconsistent Outputs       | No structure guidance                | Specify format: bullet list, JSON, numbered steps, etc.      |
| Hallucinated Facts         | Insufficient or unclear context      | Provide source text or citations; reduce temperature         |
| Prompt Overload            | Asking too much at once              | Split into separate tasks or stages                          |
| Loss of Context (Multi-Turn)| Untracked prior messages            | Add summaries or memory tokens; restate prior inputs         |

---

## 4. System Prompt Strategies

System prompts define the AI's overall persona and behavior across a session.

**Examples:**

- “You are a helpful, neutral technical support representative. Speak clearly and use concise formatting.”
- “Act as a financial analyst. Provide structured reasoning, cite data sources, and avoid speculation.”

### Best Practices

- Keep it under 1,000 characters for most models  
- Use declarative tone (e.g., "You are...")  
- Include task boundaries (“Do not provide legal advice”)

---

## 5. Role-Based Prompting Patterns

Tailor prompts to different business roles and objectives:

| Department         | Pattern Example                                                                 |
|--------------------|---------------------------------------------------------------------------------|
| **Sales**          | “Create a persuasive one-paragraph summary of this product’s key benefits.”     |
| **Support**        | “Analyze this error log and provide 3 likely causes and resolution steps.”      |
| **Legal**          | “Review this contract excerpt for NDA violations. Highlight potential issues.” |
| **Marketing**      | “Write a friendly email campaign in under 100 words targeting B2B IT buyers.”   |
| **Engineering**    | “Explain this code block in plain English with inline comments.”                |

---

## 6. Multi-Turn Interactions

When prompts span multiple steps or messages, manage context explicitly:

### Example Flow:

1. **Prompt:** "Summarize the meeting transcript below."  
2. **User:** "Now convert that summary into an executive report."  
3. **Follow-up:** "Add an action item list at the end."

**Techniques:**

- Summarize or recap in each turn  
- Store key variables in memory or state tokens  
- Ask the model to “repeat your current understanding”

---

## 7. Evaluation and Refinement Techniques

### Criteria for Reviewing Prompt Output:

| Metric        | What to Check                                           |
|---------------|---------------------------------------------------------|
| Accuracy       | Are facts preserved from the input?                    |
| Relevance      | Is the output aligned to the task?                     |
| Format         | Does it match the required structure (list, table, etc.)? |
| Tone           | Is it appropriate for the audience and role?           |
| Brevity        | Is it as concise as requested?                         |

### Refinement Tactics:

- Use few-shot examples to demonstrate expected outputs  
- Lower temperature (e.g., 0.2–0.3) for deterministic results  
- Add constraints like “Limit to 3 bullet points”  
- Use step-by-step prompting (e.g., Chain-of-Thought)

---

## 8. Use Case Templates

### 🧩 Instructional Template: Summarize  
Summarize the following [content] in no more than 100 words. Return in bullet format, grouped by topic.

### 💬 Customer Support Triage  
You are a support agent. Analyze this support ticket and assign a category (billing, technical, usability). Then draft a 2-sentence response.

### 📊 Report Generator  
Generate a weekly status report using the data below. Include sections for completed tasks, blockers, and next steps. Return in markdown.

### 🛡️ Risk Assessment  
You are a cybersecurity analyst. Review the network configuration and list 3 security risks, each with a severity score and remediation step.

---

## 9. Governance and Risk Considerations

Prompt engineering at scale must include operational and compliance guardrails:

### Prompt Metadata Schema:

| Field                  | Description                            |
|------------------------|----------------------------------------|
| `name`                | Unique prompt identifier               |
| `author`              | Person or team who created the prompt  |
| `use_case`            | What business problem this solves      |
| `last_updated`        | Date of last revision                  |
| `expected_output_format` | JSON, list, paragraph, table         |
| `risk_level`          | Low, Medium, High                      |

### Risks to Monitor:

- Hallucinations in high-stakes scenarios (legal, medical)  
- Inconsistent or offensive tone  
- Prompt misuse (e.g., bypassing policy filters)

---

## Appendix A: Prompt Debugging Checklist

- [ ] Instruction is clear and unambiguous  
- [ ] Output format is defined (bullet list, JSON, etc.)  
- [ ] Constraints (length, tone) are explicit  
- [ ] Prompt includes relevant context or examples  
- [ ] Output is consistent across multiple runs  
- [ ] System prompt defines role or function  
- [ ] Prompt works with low temperature settings (≤ 0.3)  

---

## Appendix B: Reference Links

- [OpenAI Prompting Guide](https://platform.openai.com/docs/guides/prompt-engineering)  
- [Anthropic Claude Prompting Tips](https://www.anthropic.com/index/prompting-claude)  
- [Prompt Engineering Patterns](https://github.com/dair-ai/Prompt-Engineering-Guide)  
- [LangChain Cookbook (for developers)](https://github.com/hwchase17/langchain-cookbook)  
- [Enterprise RAG Implementation Guide](https://github.com/CRollins6020/CRollins6020/blob/main/User-Guides/Enterprise%20Prompt%20Engineering%20Handbook.md)

---
