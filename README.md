# amazon-price-product-quality-analysis
##Measuring the relationship between discounts, customer satisfaction and product quality
> “A promotion should not serve as a cover-up for a poor-quality product. If a product appears to rely heavily on discounts to attract customer engagement, it is essential to identify the underlying issues through customer reviews and determine what improvements should be made.”
> > **« Une promotion ne doit pas masquer la mauvaise qualité d'un produit. Lorsqu'un article s'appuie fortement sur des remises pour attirer les clients, il est essentiel de vérifier si ces promotions s'accompagnent d'une réelle satisfaction quant à la qualité reçue ou si, au contraire, elles ne font que dissimuler des défauts sous-jacents.**
> 
> **Notre analyse vise ainsi à étudier les avis clients afin de déterminer si une évaluation élevée traduit un véritable rapport qualité-prix ou un simple "effet d'aubaine" lié à la baisse du tarif, qui masquerait la valeur réelle du produit. »**
## 🛠️ 2. Data Preparation & Data Modeling (Power Query & Power BI)

### Data Cleaning & Transformation
- **`reviews` Table:** Unpivoted nested text attributes, split concatenated strings using delimiters, and cleaned `null` values.
- **Data Standardizations:** Ensured correct data types across product identifiers, numerical pricing, discount percentages, and customer feedback fields.

### Data Model Architecture
- **Star Schema:** Built a clean Star Schema connecting the `Products` dimension table with the `reviews` fact table.
- **Relationships:** Established a 1-to-many (`1:*`) relationship on `product_id` with single-direction filtering.
- **Product Hierarchy:** Designed a 4-level hierarchy to enable seamless drill-down analysis:  
  `Main category` ➡️ `Product family` ➡️ `product types` ➡️ `product_name`
