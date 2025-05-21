
# 🧾 Dev Writing Guidelines

*Documentation standards for engineers and technical writers contributing to internal content. Includes voice/tone recommendations, formatting tips, reusable content patterns, and sample templates.*

| **Field**       | **Value**                     |
|------------------|-------------------------------|
| **Version**      | 1.0                           |
| **Author**       | Corey Rollins                |
| **Last Updated** | May 21, 2025                 |
| **Status**       | Production                    |
| **Source**       | GitHub Repository (TBD)       |

---

## Table of Contents

1. [Purpose](#1-purpose)  
2. [Voice and Tone](#2-voice-and-tone)  
3. [Formatting & Structure](#3-formatting--structure)  
4. [Content Patterns](#4-content-patterns)  
5. [Sample Templates](#5-sample-templates)  
6. [Reusable Snippets](#6-reusable-snippets)  

---

## 1. Purpose

This guide ensures consistent, high-quality internal documentation across engineering and technical writing teams. It outlines clear standards for structure, tone, and formatting—supporting reusability and cross-team clarity.

[🔝 Back to top](#table-of-contents)

___

## 2. Voice and Tone

Use a professional, direct tone:

- ✅ **Do:** “Configure the API by updating the environment variable.”
- ❌ **Don't:** “Our amazing tool lets you magically integrate your service.”

Guidelines:

- Prefer active voice  
- Use plain, domain-relevant language  
- Explain the **why** as well as the **how**  
- Clarify complexity without oversimplifying  

[🔝 Back to top](#table-of-contents)

___

## 3. Formatting & Structure

Use a modular, numbered format for ease of reuse and readability.

| Element             | Format                                    |
|:--------------------|:-------------------------------------------|
| **Headings**        | Numbered H1–H4, consistent nesting         |
| **Metadata Table**  | Top of doc or page 2                      |
| **Tables**          | Header rows only; avoid merged cells      |
| **Code blocks**     | Fenced, labeled, copyable, with context   |

Example code block:
```python
# Validate user input
if not user_data:
    raise ValueError("Missing input")
```

[🔝 Back to top](#table-of-contents)

___

## 4. Content Patterns

Use these content patterns to improve consistency:

| Pattern            | Purpose                                 |
|:-------------------|:------------------------------------------|
| **Overview**       | High-level intro with context            |
| **Steps**          | Use numbered lists for clear guidance    |
| **Tips/Notes**     | Use callouts for emphasis or reminders   |
| **Expected Result**| Close each procedure when applicable     |

💡 **Tip:** Keep procedural steps short and scannable—1–2 lines max per step.

[🔝 Back to top](#table-of-contents)

___

## 5. Sample Templates

**🧰 KB Article**

<!--
```markdown
# 🔧 <Title of KB>
*<Short summary of task or problem>*

| Field | Value |
|-------|-------|
| Version | x.x |
| Author | Your Name |
| Last Updated | Month Day, Year |
| Status | Draft |
| Source | [Repo link] |

## 1. Overview

Brief explanation of task or problem.

## 2. Steps

1. Step one  
2. Step two  
3. Step three  

💡 Tip: Restart service after changes.

## 3. Troubleshooting

Describe common errors and fixes.
```
-->

**📄 API Method Block**

<!--
```markdown
### `POST /users/create`

Create a new user in the system.

**Request Body**
```json
{
  "email": "user@example.com",
  "role": "admin"
}
```

**Response**
```json
{
  "status": "success",
  "id": "12345"
}
```
```
-->

[🔝 Back to top](#table-of-contents)

___

## 6. Reusable Snippets

Label snippets and reuse consistently:

- ✅ `prompt-template-basic.md`
- ✅ `auth-error-code-table.md`
- ✅ `deployment-checklist.md`

Store in a central `/docs/components` folder in your documentation repo.

[🔝 Back to top](#table-of-contents)

---

✅ **You’re ready to standardize your internal docs.**  
Use this guide as your source of truth and revisit quarterly for updates.
