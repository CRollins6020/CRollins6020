# 🧾 Automating PDF Generation with Documint

*Generate dynamic, professional PDFs using open-source templates and JSON data.*

| **Field**       | **Value**                         |
|----------------|-----------------------------------|
| **Version**     | 1.0                               |
| **Author**      | Corey Rollins                    |
| **Last Updated**| May 21, 2025                     |
| **Status**      | Draft                             |
| **Source**      | [GitHub Repository](https://github.com/DocumintAI/documint) |

---

## Table of Contents

1. [Overview](#1-overview)  
2. [Use Cases](#2-use-cases)  
3. [Installation](#3-installation)  
4. [Basic Usage](#4-basic-usage)  
5. [Template Configuration](#5-template-configuration)  
6. [Example Output](#6-example-output)  
7. [Troubleshooting](#7-troubleshooting)  

---

## 1. Overview

**Documint** is an open-source PDF automation tool that transforms structured data (like JSON) into formatted, branded documents using customizable templates. It's ideal for invoices, reports, onboarding packets, and more.

💡 **Tip:** Think of Documint as a developer-friendly alternative to traditional document tools like Adobe Acrobat or MS Word Mail Merge.

---

## 2. Use Cases

| **Scenario**                     | **Benefit**                                     |
|----------------------------------|-------------------------------------------------|
| Invoice generation               | Automate billing for eCommerce or SaaS         |
| Internal reporting               | Standardize recurring metrics reports           |
| HR onboarding                    | Generate personalized offer letters at scale    |
| Form submissions                 | Format and archive intake data from web apps    |

---

## 3. Installation

Ensure [Node.js](https://nodejs.org/) is installed.

```bash
npm install -g documint
```

Clone the repo to access templates and scripts:

```bash
git clone https://github.com/DocumintAI/documint.git
cd documint
```

---

## 4. Basic Usage

Run the CLI with your data and template:

```bash
documint generate \
  --template ./templates/invoice.html \
  --data ./data/customer.json \
  --output ./pdf/invoice-001.pdf
```

---

## 5. Template Configuration

Documint templates use standard HTML with `{{handlebars}}` syntax to inject values.

**Example Snippet**:

```html
<h1>Invoice #{{invoice_id}}</h1>
<p>Customer: {{customer_name}}</p>
<p>Amount Due: ${{amount}}</p>
```

📁 Recommended structure:

```
/templates/
  invoice.html
/data/
  customer.json
/pdf/
  output.pdf
```

💡 **Tip:** Style templates with CSS for branded output.

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

**Resulting PDF:**

> A clean, styled invoice labeled "Invoice #001" with Acme Corp's name and due amount.

---

## 7. Troubleshooting

| **Issue**                         | **Solution**                                                  |
|----------------------------------|---------------------------------------------------------------|
| Output file is blank             | Check for missing or misspelled Handlebars variables          |
| Styles not applied               | Ensure CSS is inline or embedded in the HTML template         |
| CLI not found                    | Verify installation and check `$PATH` for global access       |

---

✅ Documint is a practical, scriptable tool for automating document generation—perfect for modern, API-connected workflows.
