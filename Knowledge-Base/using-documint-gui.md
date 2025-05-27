# 🖼️ Creating a PDF with the Documint GUI

**Applies to:** Documint Web App  
**Audience:** Business Users, QA, Analysts  
**Updated:** May 26, 2025  
**Difficulty:** Beginner  
**Contact:** support@example.com

---

## 🛠️ Problem

You need to generate a professional PDF—such as an invoice, certificate, or report—without writing code. You're using the Documint web interface but unsure how to upload templates, bind data, and export the final PDF.

---

## ✅ Solution

This article walks you through generating a PDF with Documint's no-code graphical interface using pre-built templates and JSON-formatted data.

---

## 🧩 Prerequisites

- A Documint account with template access  
- A published template with dynamic fields (e.g., `{{customer.name}}`)  
- JSON data ready to upload  
- A modern browser (Chrome, Edge, Safari)

---

## 🪜 Steps

### 1. Log In and Access Your Template

1. Visit [https://app.documint.me](https://app.documint.me)  
2. Log in with your credentials  
3. Navigate to the **Templates** tab  
4. Select your published template from the list

🎯 **You should see:** The template preview and a button labeled **Generate PDF**

---

### 2. Upload JSON Data

1. Click the **Generate PDF** button  
2. In the pop-up window, click **Upload JSON**  
3. Choose or drag in your `.json` file

**Example:**
```json
{
  "customer": {
    "name": "Jordan Miles",
    "email": "jmiles@example.com"
  },
  "invoice": {
    "number": "INV-1024",
    "total": "$3,200"
  }
}
```

✅ Documint will validate the JSON keys against the dynamic fields in your template.

---

### 3. Generate and Download the PDF

1. Click **Generate**  
2. Wait for the preview to render (usually <10 seconds)  
3. Click **Download PDF** to save your file

🎯 **You should see:** A clean, styled document with your data filled in

---

## 🔍 If You Hit a Problem

| Symptom                            | Likely Cause                        | Resolution                                  |
|------------------------------------|-------------------------------------|---------------------------------------------|
| Fields show as `{{field.name}}`    | Data binding failed                 | Check that keys in JSON match template tags |
| No preview renders                 | Invalid JSON format                 | Use a JSON linter or validator              |
| Downloaded PDF is blank            | Template was saved without content  | Re-edit and publish the template            |

---

## 📬 Support Escalation Template

```
Template Name: <e.g., Invoice_V1>
Data File: <e.g., invoice1024.json>
Steps Taken: Upload → Generate → Download
Observed Issue: <description>
Browser & OS: <e.g., Chrome 125 on Windows 11>
```

---

## 📚 Related Resources

- [Documint CLI Automation KB](https://kb.example.com/documint-cli-pdf)  
- [Documint Template Designer Guide](https://kb.example.com/template-designer)  
- [JSON Formatting Tips](https://www.jsonlint.com)

---

*Article ID: KB-DOC-GUI-001 | Updated: May 26, 2025*
