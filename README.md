# 360-cockpit
# 📊 Olist Brazilian E-Commerce Analytics Portfolio

![Dashboard Overview](assets/dashboard_preview.png)

## 🔗 Live Interactive Links & Downloads
- 🌐 **Interactive Power BI Service Link:** [View Live Dashboard](YOUR_POWER_BI_PUBLISH_TO_WEB_LINK_HERE)
- 💾 **Download Full .PBIX File:** [Download via Google Drive / OneDrive](YOUR_CLOUD_STORAGE_LINK_HERE)

---

## 📌 Executive Summary
This project delivers an end-to-end Power BI analytics solution analyzing **100k+ Brazilian e-commerce orders** from Olist (2016–2018). The dashboard equips stakeholders with actionable insights across three core business domains: **Executive Sales Performance**, **Logistics & Regional Delivery Optimization**, and **Customer Satisfaction & Review Analysis**.

---

## 🏗 Data Architecture & Modeling

* **Schema Pattern:** Clean **Star Schema** centered on core entities (`Orders`, `Order Items`, `Customers`, `Products`, and `Reviews`).
* **Filter Propagation:** Model propagation relies on key downstream relationships:
  $$\text{Customers} \longrightarrow \text{Orders} \longleftrightarrow \text{Order Items} \longrightarrow \text{Reviews}$$

### 🛠 Key Technical & DAX Solutions
1. **Dynamic Context Filtering (`CROSSFILTER`):** Resolved multi-table circular dependencies and cross-filtering issues between Product Categories and Reviews without altering global model cardinality by using inline `CROSSFILTER` DAX patterns:
   ```dax
   Avg_Review_Score = 
   CALCULATE(
       AVERAGE(olist_order_reviews_dataset[review_score]),
       CROSSFILTER(olist_orders[order_id], olist_order_items_dataset[order_id], BOTH)
   )



   Avg_Freight_Cost_By_State = 
AVERAGEX(
    VALUES(olist_customers_dataset[customer_id]),
    CALCULATE(SUM(olist_order_items_dataset[freight_value]))
)
