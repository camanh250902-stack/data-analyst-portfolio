## Executive Summary
This analysis examines Olist's e-commerce order data from September 2016 to October 2018, covering roughly 100,000 orders across customers, products, sellers, and reviews. The goal was to understand the health of the business along four dimensions — revenue growth, category performance, delivery's effect on customer satisfaction, and regional supply gaps — and to surface specific, actionable recommendations rather than just describe the data.

**Description of source of data:**<br>
I chose the Brazilian-eCommerce dataset from Kaggle for the analysis. This dataset contains approximately 100,000 customer orders, along with corresponding files on product information and English translations of product categories originally in Portuguese. Seller names in this dataset were anonymized and replaced with Game of Thrones House names. Nine files from the original Kaggle dataset were chosen for further analysis: olist_geolocation_dataset, olist_customers_dataset, olist_sellers_dataset, olist_product_dataset, olist_order_items_dataset, olist_orders_dataset, olist_order_payments_dataset, olist_order_reviews_dataset and product_category_name_translation.

Data Source: https://www.kaggle.com/olistbr/brazilian-ecommerce

**Data Overview**<br>
The analysis draws on 8 of the 9 CSV files in the Olist Brazilian E-Commerce dataset, merged into a single working table via order_id, product_id, customer_id, and seller_id:

**olist_orders_dataset.csv** — order-level status and timestamps (purchase, approval, delivery)<br>
**olist_order_items_dataset.csv** — line-item detail: product, seller, price, freight<br>
**olist_products_dataset.csv** — product category and attributes<br>
**olist_customers_dataset.csv** — customer location (state, city)<br>
**olist_order_reviews_dataset.csv** — review scores and comments<br>
**olist_sellers_dataset.csv** — seller location (state, city)<br>
**olist_order_payments_dataset.csv** — payment type and installment info<br>
**product_category_name_translation.csv** — maps Portuguese category names to English<br>

Only olist_geolocation_dataset.csv (zip-code-level lat/long) wasn't used for these 4 questions, but it's available for follow-up analysis — e.g. mapping the underserved states from finding #4 geographically. After merging, the working table covers ~100,000 orders (with the missing-value handling and cleaning steps documented in the notebook).

**Questions we hope to answer with our data:**<br>
Is the business growing, and is that growth steady or driven by isolated spikes?
Which product categories generate the most revenue, and does that match which categories sell the most units?
Does delivery speed measurably affect how customers rate their experience?
Are there regions where demand for products isn't being matched by local seller supply?

**1. Revenue Growth**<br>
The first chart shows total revenue summed by month, from September 2016 through October 2018, plotted as a single trend line. Monthly revenue climbed from near-zero in late 2016 to a peak of ~R$1.05M in November 2017, then held in the R$0.8M-1.0M range through mid-2018 — evidence of a maturing, stabilizing business rather than one still in early hockey-stick growth. The sudden drop to zero in the final two months is very likely an artifact of the dataset's collection cutoff, not a real business collapse — worth stating explicitly in the README so a reader doesn't misread it as a crisis.
Recommendation: exclude the final 1-2 months from any growth-rate calculation (e.g. month-over-month %), since including them would make a healthy business look like it fell off a cliff.
<img width="1184" height="484" alt="image" src="https://github.com/user-attachments/assets/ba439f3f-1d31-44c2-af74-faedcfc9a8c2" />

**2. Category Performance**<br>
Two side-by-side bar charts rank the top 10 product categories — one by total revenue, the other by number of orders — so categories that sell a lot but generate little revenue (and vice versa) stand out by comparing their position across both charts. bed_bath_table has the highest order count of any category but ranks only third in revenue, while watches_gifts ranks second in revenue despite fewer total orders than five other categories — consistent with watches_gifts selling fewer, higher-priced items, and bed_bath_table selling more, cheaper ones.
Recommendation: treat these as two different growth levers — watches_gifts is a margin/high-ticket play worth protecting inventory for, while bed_bath_table's volume suggests fulfillment/logistics capacity is what actually caps its revenue ceiling, not demand.
<img width="1384" height="584" alt="image" src="https://github.com/user-attachments/assets/504b9129-ec7e-484b-95ff-0ad5ce451806" />

**3. Delivery & Satisfaction**<br>
Orders are grouped into four delivery-time buckets (0-3, 4-7, 8-14, and 15+ days), and two charts show the relationship with review scores: a bar chart of the average score per bucket, and a boxplot showing the full score distribution (median, spread, and outliers) within each bucket. Average review score drops steadily from 4.39 (0-3 day delivery) to 3.6 (15+ days). The boxplot adds an important detail the average hides: orders delivered in under 14 days are tightly clustered near 4-5 stars, but the 15+ day bucket has a much wider spread, with the median itself dropping to 4 and a heavier concentration of 1-2 star outliers — meaning slow delivery doesn't just lower the average, it makes outcomes unpredictable.
Recommendation: prioritize cutting the tail of slowest deliveries (15+ days) specifically, rather than trying to shave a day or two off the already-fast majority — that's where the rating risk is concentrated.
<img width="784" height="484" alt="image" src="https://github.com/user-attachments/assets/98cd153e-6ea6-46f9-b30a-471de85ae0bb" />

**4. Regional Coverage**<br>
This chart ranks the 10 states with the highest ratio of customer orders to locally based sellers, as a proxy for which regions have demand that isn't being met by nearby supply. PA's orders-per-seller ratio (975) is more than 30% higher than the next-highest state (MA, ~745), and roughly 5x higher than the states at the bottom of this top-10 list — meaning demand in PA is being served almost entirely by sellers based elsewhere.
Recommendation: PA is the clearest candidate for local seller recruitment — the demand is proven, the local supply is the gap, and closing it would likely also improve delivery times (and therefore review scores, per finding #3) for that region specifically.

<img width="881" height="584" alt="image" src="https://github.com/user-attachments/assets/82ef92be-8140-421e-b918-ad55f39ac1d9" />



Taken together, these findings point to a business that has grown past its early-stage phase and is now optimizing rather than just scaling. The clearest next steps are operational, not strategic: reduce the tail of slow deliveries where satisfaction risk is concentrated, recruit sellers in underserved states like PA to close the supply gap driving those slow deliveries in the first place, and treat high-margin categories like watches_gifts differently from high-volume ones like bed_bath_table when planning inventory and logistics investment. None of these require new markets or new products — they're improvements to how the current business is being run.
