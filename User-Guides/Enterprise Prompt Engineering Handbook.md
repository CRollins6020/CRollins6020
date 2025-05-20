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

[Back to Top](#enterprise-prompt-engineering-handbook)

---

## 2. Prompt Design Fundamentals

A well-engineered prompt includes:

| Component       | Description                              | Example                                              |
|----------------|------------------------------------------|------------------------------------------------------|
| **Instruction** | What the model should do                 | “Summarize this text in 3 bullet points.”            |
| **Input**       | The source content                       | “Customer chat log, JSON payload, markdown document” |
| **Constraints** | Limits or boundaries                     | “Use no more than 150 words. Exclude pricing details.” |
| **Format**      | Expected structure of the output         | “Respond in markdown table format with 3 columns.”   |
| **Tone/Role**   | The model’s persona or perspective       | “You are a professional cybersecurity analyst.”      |

> **Tip:** Start simple. Test incrementally with clear constraints before adding complexity.

### Example
<details>
<summary>🎯 Click to expand</summary>

You are a senior data analyst. Summarize the customer churn report below in three key findings. Respond with a numbered list and no more than 75 words.

</details>

[Back to Top](#enterprise-prompt-engineering-handbook)

---

## 3. Common Pitfalls and How to Avoid Them

| Pitfall                  | Cause                          | Fix                                                        |
|--------------------------|--------------------------------|-------------------------------------------------------------|
| Vague Instructions       | Ambiguous verbs or goals       | Use direct, imperative phrasing                             |
| Inconsistent Outputs     | No structure guidance          | Specify format: bullet list, JSON, numbered steps, etc.     |
| Hallucinated Facts       | Insufficient or unclear context| Provide source text or citations; reduce temperature        |
| Prompt Overload          | Asking too much at once        | Split into separate tasks or stages                         |
| Loss of Context (Multi-Turn) | Untracked prior messages   | Add summaries or memory tokens; restate prior inputs        |

### Bad Prompt
<details>
<summary>⚠️ Bad Example</summary>

Tell me about this data.

</details>

### Improved Prompt
<details>
<summary>✅ Better Example</summary>

You are a financial analyst. Review the sales data below and list 3 insights related to regional performance. Output as a markdown bullet list.

</details>

[Back to Top](#enterprise-prompt-engineering-handbook)

---

## 4. System Prompt Strategies

System prompts define the AI's overall persona and behavior across a session.

### Examples
<details>
<summary>🧠 Persona Examples</summary>

- You are a helpful, neutral technical support representative. Speak clearly and use concise formatting.  
- Act as a financial analyst. Provide structured reasoning, cite data sources, and avoid speculation.

</details>

### Best Practices

- Keep under 1,000 characters  
- Use declarative tone  
- Define boundaries clearly  

[Back to Top](#enterprise-prompt-engineering-handbook)

---

## 5. Role-Based Prompting Patterns

Tailor prompts to different business roles and objectives:

| Department     | Pattern Example                                                              |
|----------------|-------------------------------------------------------------------------------|
| **Sales**      | “Create a persuasive one-paragraph summary of this product’s key benefits.”   |
| **Support**    | “Analyze this error log and provide 3 likely causes and resolution steps.”    |
| **Legal**      | “Review this contract excerpt for NDA violations. Highlight potential issues.”|
| **Marketing**  | “Write a friendly email campaign in under 100 words targeting B2B IT buyers.” |
| **Engineering**| “Explain this code block in plain English with inline comments.”              |

### Example
<details>
<summary>💼 Engineering Prompt</summary>

You are a solutions engineer. Explain the following YAML configuration to a non-technical customer success manager.

</details>

[Back to Top](#enterprise-prompt-engineering-handbook)

---

## 6. Multi-Turn Interactions

When prompts span multiple steps or messages, manage context explicitly.

### Conversation Flow Example
<details>
<summary>🗣️ Multi-Turn Sequence</summary>

1. [User] Summarize the meeting transcript below.  
2. [AI] (Provides summary)  
3. [User] Now convert that summary into an executive report.  
4. [AI] (Converts summary)  
5. [User] Add a section listing follow-up action items.

</details>

### Tips

- Recap prior input  
- Repeat “your understanding so far”  
- Store relevant details in variables  

[Back to Top](#enterprise-prompt-engineering-handbook)

---

## 7. Evaluation and Refinement Techniques

### Criteria for Reviewing Prompt Output

| Metric     | What to Check                                      |
|------------|----------------------------------------------------|
| Accuracy   | Are facts preserved from the input?                |
| Relevance  | Is the output aligned to the task?                 |
| Format     | Does it match the required structure?              |
| Tone       | Is it appropriate for the audience?                |
| Brevity    | Is it concise and to the point?                    |

### Refinement Example
<details>
<summary>🔧 Fix This Prompt</summary>

**Instead of:**  
Write an overview of the attached document.

**Try:**  
Summarize the attached document in 4 bullet points. Focus on risks, deadlines, and stakeholders. Limit output to 75 words.

</details>

[Back to Top](#enterprise-prompt-engineering-handbook)

---

## 8. Use Case Templates

### 🧩 Summarize
<details>
<summary>Prompt Template</summary>

You are a training specialist. Summarize the following transcript in under 100 words. Use 3 bullet points grouped by topic.

</details>

### 💬 Support Ticket Triage
<details>
<summary>Prompt Template</summary>

You are a support agent. Classify this ticket as billing, technical, or usability. Then write a two-sentence reply to the user.

</details>

### 📊 Weekly Report Generator
<details>
<summary>Prompt Template</summary>

Generate a weekly report using the data below. Include Completed Tasks, Blockers, and Next Steps. Return in markdown.

</details>

### 🛡️ Risk Assessment
<details>
<summary>Prompt Template</summary>

You are a cybersecurity analyst. Review the firewall configuration and list 3 security risks. Include a severity rating and suggested fix.

</details>

[Back to Top](#enterprise-prompt-engineering-handbook)

---

## 9. Governance and Risk Considerations

Prompt engineering at scale requires accountability, versioning, and compliance controls.

### Metadata Schema

| Field                   | Description                            |
|-------------------------|----------------------------------------|
| `name`                  | Unique prompt identifier               |
| `author`                | Person or team who created the prompt  |
| `use_case`              | What business problem this solves      |
| `last_updated`          | Date of last revision                  |
| `expected_output_format`| JSON, list, paragraph, table           |
| `risk_level`            | Low, Medium, High                      |

### Risks to Monitor

- Hallucinations in regulated domains  
- Mismatched tone for the audience  
- Prompts used to bypass content restrictions  

[Back to Top](#enterprise-prompt-engineering-handbook)

---

## Appendix A: Prompt Debugging Checklist

- [ ] Instruction is clear and unambiguous  
- [ ] Output format is defined (bullet list, JSON, etc.)  
- [ ] Constraints (length, tone) are explicit  
- [ ] Prompt includes relevant context or examples  
- [ ] Output is consistent across multiple runs  
- [ ] System prompt defines role or function  
- [ ] Prompt works with low temperature settings (≤ 0.3)  

[Back to Top](#enterprise-prompt-engineering-handbook)

---

## Appendix B: Reference Links

- [OpenAI Prompting Guide](https://platform.openai.com/docs/guides/prompt-engineering)  
- [Anthropic Claude Prompting Tips](https://www.anthropic.com/index/prompting-claude)  
- [Prompt Engineering Patterns](https://github.com/dair-ai/Prompt-Engineering-Guide)  
- [LangChain Cookbook (for developers)](https://github.com/hwchase17/langchain-cookbook)  
- [Enterprise RAG Implementation Guide](https://github.com/CRollins6020/CRollins6020/blob/main/User-Guides/Enterprise%20Prompt%20Engineering%20Handbook.md)

[Back to Top](#enterprise-prompt-engineering-handbook)

---
