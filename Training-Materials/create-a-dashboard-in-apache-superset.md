# 🎓 Create a Dashboard in Apache Superset
*Learn how to create a functional data dashboard using the open-source Apache Superset platform.*

| **Field**         | **Value**                                |
|-------------------|--------------------------------------------|
| **Version**       | 1.0                                       |
| **Author**        | Corey Rollins                            |
| **Last Updated**  | May 21, 2025                             |
| **Status**        | Draft                                     |
| **Audience**      | New Users, Data Analysts                  |
| **Estimated Time**| ~20–30 min                                |

---

## 🧠 Learning Objectives

By the end of this module, you will be able to:

- ✅ Log in to Apache Superset and navigate the interface
- ✅ Create and customize charts in the Explore view
- ✅ Combine multiple charts into a single interactive dashboard

---

## 1. Introduction

Apache Superset is a powerful, open-source platform for data exploration and visualization. In this hands-on module, you’ll walk through the process of creating a basic dashboard using a sample dataset.

No prior Superset experience is required.

---

## 2. Use Case Scenario

**Scenario:**  
You’re a business analyst who needs to monitor product performance by country. Your task is to create a dashboard using the `product_sales` dataset to visualize total revenue by region and product category.

---

## 3. Step-by-Step Walkthrough

### Step 1 – Log In and Access Datasets

1. Visit your Superset URL (e.g., `https://superset.company.com`)
2. Log in using SSO or your assigned credentials
3. From the left menu, go to **Data** → **Datasets**
4. Locate and select `product_sales`

💡 *Tip:* Use the search bar if your dataset list is long.

---

### Step 2 – Create Your First Chart

1. Click **Explore** next to the `product_sales` dataset
2. In the Explore view:
    - Select **Bar Chart** as the visualization type
    - Set **X-axis** to `country`
    - Set **Y-axis** to `SUM(revenue)`
3. Click **Run** to preview the chart
4. Use the **Save** button to name your chart:  
   📄 _“Revenue by Country”_

🖼️ *(Screenshot placeholder: explore-view-bar-chart.png)*

---

### Step 3 – Create a Second Chart

1. Click the Superset logo → return to **Datasets**
2. Click **Explore** on `product_sales` again
3. This time:
    - Use **Pie Chart**
    - Set **Dimension** to `product_category`
    - Set **Metric** to `SUM(revenue)`
4. Run and save this chart as:  
   📄 _“Revenue by Product Category”_

---

### Step 4 – Build a Dashboard

1. Go to **Dashboards** → click **+ Dashboard**
2. Give it a title:  
   🧾 _“Product Performance Dashboard”_
3. Add your two charts to the dashboard canvas:
    - Drag from **Your Charts** into the layout
4. Arrange charts side-by-side or in a column

⚠️ *Warning:* Unsaved dashboards will be lost on refresh. Click **Save** often.

---

### Step 5 – Add Filters (Optional)

1. Click **+ Filter Box**
2. Configure filter for `country` or `date`
3. Save and test filter functionality on your dashboard

---

## 4. Knowledge Check

> 🧠 *Which of the following is required to display data in a Superset chart?*  
> A. A filter set  
> B. A dataset connection  
> C. A saved dashboard  
> D. An export token  

✔️ **Correct Answer: B. A dataset connection**

---

## 5. Summary and Next Steps

You’ve created two charts and combined them into an interactive dashboard using Apache Superset. This is a key foundational skill for data exploration and reporting in any organization.

✅ Next Steps:
- Try adding more visualizations (e.g., line charts or tables)
- Experiment with dashboard filters and layout options
- Share your dashboard with teammates or stakeholders

---

## 🔗 Resources

- [Official Superset Docs](https://superset.apache.org/docs/intro)
- [Explore Chart Types](https://superset.apache.org/docs/creating-charts-dashboards/explore)
- [Dashboard Layout Tips](https://superset.apache.org/docs/creating-charts-dashboards/dashboards)
