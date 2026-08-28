# amazon-price-product-quality-analysis
##Measuring the relationship between discounts, customer satisfaction and product quality
> “A promotion should not serve as a cover-up for a poor-quality product. If a product appears to rely heavily on discounts to attract customer engagement, it is essential to identify the underlying issues through customer reviews and determine what improvements should be made.”
## 🛠️ 2. Data Preparation & Data Modeling (Power Query & Power BI)

### Data Cleaning & Transformation
- **`reviews` Table:** Unpivoted nested text attributes, split concatenated strings using delimiters, and cleaned `null` values.
- **Data Standardizations:** Ensured correct data types across product identifiers, numerical pricing, discount percentages, and customer feedback fields.

### Data Model Architecture
- **Star Schema:** Built a clean Star Schema connecting the `Products` dimension table with the `reviews` fact table.
- **Relationships:** Established a 1-to-many (`1:*`) relationship on `product_id` with single-direction filtering.
- **Product Hierarchy:** Designed a 4-level hierarchy to enable seamless drill-down analysis:  
  `Main category` ➡️ `Product family` ➡️ `product types` ➡️ `product_name`
