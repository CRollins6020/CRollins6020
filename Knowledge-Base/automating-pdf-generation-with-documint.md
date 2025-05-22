# 🧾 Automating PDF Generation with Documint  
*Generate dynamic, professional PDFs using open-source templates and JSON data.*

**A developer guide to JSON-driven PDF automation.**

| **Field**        | **Value**                                                                 |
|------------------|--------------------------------------------------------------------------|
| **Version**      | 1.0                                                                      |
| **Author**       | Corey Rollins                                                            |
| **Last Updated** | May 21, 2025                                                             |
| **Status**       | Draft                                                                    |
| **Source**       | [GitHub Repository](https://github.com/DocumintAI/documint)              |

---

## Table of Contents

1. [Overview](#1-overview)  
2. [Use Cases](#2-use-cases)  
3. [Installation](#3-installation)  
4. [Basic Usage](#4-basic-usage)  
5. [Template Configuration](#5-template-configuration)  
6. [Example Output](#6-example-output)  
7. [Troubleshooting](#7-troubleshooting)  
8. [See Also](#8-see-also)  

---

## 1. Overview

**Documint** is an open-source PDF generation tool that converts structured JSON data into professional, branded documents using HTML templates. Ideal for automated workflows like invoicing, reporting, and HR onboarding.

💡 **Tip:** Use Documint to replace manual document formatting with repeatable, scriptable processes.

[🔝 Back to top](#table-of-contents)

---

## 2. Use Cases

| **Scenario**           | **Benefit**                                |
|------------------------|---------------------------------------------|
| Invoice generation     | Automate recurring billing at scale         |
| Internal reporting     | Standardize report formatting for execs     |
| HR onboarding          | Generate personalized offer letters         |
| Form submissions       | Store and format structured form responses  |

[🔝 Back to top](#table-of-contents)

---

## 3. Installation

Ensure [Node.js](https://nodejs.org/) is installed.

```bash
npm install -g documint
```

Clone the repository to access sample templates and scripts:

```bash
git clone https://github.com/DocumintAI/documint.git
cd documint
```

[🔝 Back to top](#table-of-contents)

---

## 4. Basic Usage

Generate a PDF by combining a data file with a template:

```bash
documint generate \
  --template ./templates/invoice.html \
  --data ./data/customer.json \
  --output ./pdf/invoice-001.pdf
```

[🔝 Back to top](#table-of-contents)

---

## 5. Template Configuration

Documint uses HTML templates with `{{handlebars}}` syntax for variable injection.

**Example Snippet**:

```html
<h1>Invoice #{{invoice_id}}</h1>
<p>Customer: {{customer_name}}</p>
<p>Amount Due: ${{amount}}</p>
```

📁 Suggested project structure:

```
/templates/
  invoice.html
/data/
  customer.json
/pdf/
  output.pdf
```

💡 **Tip:** Add embedded CSS to ensure consistent branding across all output.

[🔝 Back to top](#table-of-contents)

---

## 6. Example Output

**Input JSON:**

```json
{
  "invoice_id": "001",
  "customer_name": "Acme Corp",
  "amount": "1499.00"
}
```

**Rendered Output:**  
> A clean PDF invoice labeled "Invoice #001" with Acme Corp’s name and amount due.

![Sample Invoice Output](assets/img/sample-invoice-preview.png "Sample Invoice Preview")

[🔝 Back to top](#table-of-contents)

---

## 7. Troubleshooting

| **Issue**                   | **Solution**                                                  |
|----------------------------|---------------------------------------------------------------|
| Blank output file           | Check for missing or incorrect Handlebars variables           |
| CSS not applied             | Ensure styles are inline or embedded in the HTML template     |
| CLI command not found       | Confirm global install and check `$PATH`                      |
| Unsupported characters      | Verify UTF-8 encoding and escape special HTML characters      |
| Template rendering failure  | Ensure Handlebars syntax is correct and all variables exist   |

[🔝 Back to top](#table-of-contents)

---

## 8. See Also

- [Handlebars Templating Documentation](https://handlebarsjs.com/)
- [Documint Issues & Feature Requests](https://github.com/DocumintAI/documint/issues)
- [Markdown PDF Templates (Advanced)](https://github.com/seladb/pickley)
- [Example Documint Use Case Repo](https://github.com/example-org/documint-pdf-workflows) <!-- Replace with a real one if available -->

---

✅ This article demonstrates how to install, configure, and use Documint to automate PDF generation workflows using JSON and HTML. Suitable for developers building lightweight, scalable document systems.
