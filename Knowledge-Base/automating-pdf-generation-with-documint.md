# 🧾 Automating PDF Generation with Documint

**Applies to:** Documint CLI & Web App | **Updated:** May 26, 2025  
**Difficulty:** Intermediate | **Time:** 15–30 minutes | **Support:** support@example.com

---

## 🛠️ Problem

You need to automate the generation of professional PDF documents (e.g., invoices, reports, certificates) using structured JSON data. Manual exports are time-consuming and error-prone, especially at scale.

---

## ✅ Solution

Documint allows you to dynamically generate PDFs by binding JSON data to reusable templates via CLI, API, or web app. This KB covers how to:

- Set up your PDF template
- Bind JSON data
- Generate PDFs via the CLI
- Automate batch document generation

---

## 🧰 Prerequisites

- A Documint account (Free or Pro)
- Template created in the Documint web editor
- JSON file with dynamic data
- Installed Documint CLI (`npm install -g @documint/cli`)

---

## 🧩 Steps

### 1. Create and publish your template

1. Log in to [Documint](https://app.documint.me)
2. Use the drag-and-drop editor to design your layout  
3. Add dynamic fields using handlebars syntax (e.g., `{{customer.name}}`)  
4. Publish the template and copy the **Template ID**

🎯 **You should see:** The status of the template change to “Published”

💡 **Optional:** Assign metadata like category or version to your template in the web UI for better traceability.

---

### 2. Prepare your JSON data

**Example:**
```json
{
  "customer": {
    "name": "Samantha Lee",
    "email": "samantha@example.com"
  },
  "invoice": {
    "number": "INV-2345",
    "amount": "$1,250.00"
  }
}
```

💡 **Tip:** Store dynamic entries in a `.json` file for CLI-based binding.

---

### 3. Use Documint CLI to generate the PDF

```bash
documint render   --template-id tmpl_123456789   --data ./data/invoice.json   --output ./output/invoice.pdf
```

🎯 **You should see:** `invoice.pdf` created in the output folder.

💡 **Add dynamic file naming with metadata:**
```bash
documint render \
  --template-id tmpl_123456789 \
  --data ./data/invoice.json \
  --output "./output/invoice-$(date +%Y%m%d).pdf"
```

---

### 4. Automate with scripts

Batch generation with a shell script:

```bash
for file in ./data/*.json; do
  name=$(basename "$file" .json)
  documint render \
    --template-id tmpl_123456789 \
    --data "$file" \
    --output "./output/$name.pdf"
done
```

🎯 **You should see:** Multiple PDFs generated for each JSON file.

---

## 🔍 Verify It Worked

1. Open several PDFs in a viewer like Adobe Reader or browser PDF preview
2. Confirm the following:
   - All fields are populated correctly  
   - Fonts, spacing, and layout match your design  
   - No `undefined` or missing data values  
   - File opens without corruption or load errors

✅ If all checks pass, you’re ready for production use.

---

## 🔁 Optional: Use the API for advanced workflows

Instead of the CLI, you can call the Documint REST API to render documents programmatically.

**Example:**
```bash
curl -X POST https://api.documint.me/render   -H "Authorization: Bearer YOUR_API_KEY"   -H "Content-Type: application/json"   -d '{
    "template_id": "tmpl_123456789",
    "data": {
      "customer": { "name": "Samantha Lee" }
    }
  }'
```

💡 **Tip:** Use the API for integration into backend services or scheduled job runners like Zapier, Make, or GitHub Actions.

---

## 🧯 What if this doesn't work?

| Problem                     | Possible Cause                  | Fix                                           |
|-----------------------------|----------------------------------|-----------------------------------------------|
| Blank PDF output            | Missing or incorrect template ID| Recheck the published template ID             |
| "Field not found" error     | JSON structure mismatch         | Match JSON keys to template field bindings    |
| CLI not recognized          | CLI not installed globally      | Run `npm install -g @documint/cli` again      |
| PDF won't open              | Corrupted output                | Check for typos in file path or data errors   |

---

📫 Still stuck? [Submit a support request](mailto:support@example.com) or visit the [Documint Help Center](https://help.documint.me)

*Article ID: KB-DOC-001 | Updated: May 26, 2025*
