# SQL_Ecommerce_Instruction
*Explore E-commerce utilizing Google BigQuery dataset, aiming to evaluate the business situation, website activity, marketing effectiveness (bounce rate per traffic,funnel analysis, cohort,..), and product analysis*
***
### Table of Content
[I. Introduction](https://github.com/ju1701/SQL_Ecommerce_Instruction/edit/Master/README.md#i-introduction)
 - Prerequisites
 - How to assess the Database
 - Dataset Dictionary
 - Dataset Structure
 - Data field
   
[II. Project objectives](https://github.com/ju1701/SQL_Ecommerce_Instruction/edit/Master/README.md#ii-project-objectives)

[III. Dataset Exploration](https://github.com/ju1701/SQL_Ecommerce_Instruction/edit/Master/README.md#iii-project-exploration)
 - Disclaim
 - Queries
***
### I. Introduction 
The project dataset comes from a free and public dataset from BigQuery, contains obfuscated Google Analytics 360 data from a real e-commerce store, which is [Google Merchandise Store](https://www.googlemerchandisestore.com/shop.axd/Home?utm_source=Partners&utm_medium=affiliate&utm_campaign=Data%20Share%20Promo).

The Google Merchandise Store sells Google-branded merchandise. The data is typical of what we would see for an e-commerce website, includes the following kinds of information:
- Traffic source data: information about where website visitors originate. This includes data about organic traffic, paid search traffic, display traffic, etc.
- Content data: information about the behavior of users on the site. This includes the URLs of pages that visitors look at, how they interact with content, etc.
- Transactional data: information about the transactions that occur on the Google Merchandise Store website.

**1. Prerequisites**

- [Google Cloud Platform account](https://cloud.google.com/)
- Project on Google Cloud Platform
- [Google BigQuery API](https://cloud.google.com/bigquery/docs/enable-transfer-service#:~:text=Enable%20the%20BigQuery%20Data%20Transfer%20Service,-Before%20you%20can&text=Open%20the%20BigQuery%20Data%20Transfer,Click%20the%20ENABLE%20button.) (Application Programming Interface) enabled
- [SQL query editor](https://cloud.google.com/monitoring/mql/query-editor) or IDE (Integrated Development Environment)

**2. How to assess the database**

- Log in to Google Cloud Platform account and create a new project.
- Navigate to the BigQuery console and select a created project.
- In the navigation panel, navigate to “Search BigQuery resources”(left), type “ga_sessions”.
- If the dataset hasn’t appeared yet, click “Broaden search to all projects”
- Star the dataset so that we can easily assess it later on our project

**3. Dataset Dictionary**

- After assessing the dataset, click on it and navigate to the “scheme” table. By this, we can navigate to know dataset fields, rows, types, and modes. (“Preview” besides “scheme” allows us to see the preview version of the dataset)

**4. Dataset structure: Array**

An **array** is a data structure that stores multiple elements of the **same data type** in a contiguous block of memory. Each element in an array is accessed using an **index**, which represents its position in the array.

   ![z3323474566789-4fffb1de04e62014616ef85f5b7e445b](https://github.com/user-attachments/assets/12d73105-2f62-49f9-a7bc-0bb34d828d1a)   
   
- Characteristics of Array

  **Fixed Size** - The size of an array is usually determined at the time of its creation and cannot be changed dynamically.

  **Contiguous Memory Allocation** – Elements are stored in consecutive memory locations, which allows fast access.

  **Indexed Access** – Each element is accessed using an index (starting from 0 in most programming languages).

  **Efficient Retrieval** – Since arrays store elements sequentially, accessing an element by index takes constant time **O(1)**.

  **Homogeneous Elements** – All elements in an array must be of the same type (e.g., integers, characters, floats).

Using big datasets that have dataset updated everyday, businesses use Array structure in dataset storage for **Efficient Memory Usage and fast assess.** 

While having its advantages, Array structure have its drawbacks for its fixed size and costly Insertion/Deletion fee as it Requires shifting elements if an item is added/removed in the middle.

**5. Data field**

Before querying, we need to navigate the dataset dictionary to understand the data scheme field description, and type of column (dimension or measure), by using the right field while querying to answer business questions. 

Here are the necessary fields in the projects. For a comprehensive data dictionary, please access [here](https://support.google.com/analytics/answer/3437719?hl=en)

| Field Name  | Datatype  | Description |
|--------|--------|--------|
| fullVisitorId | STRING | The unique visitor ID. |
| date | STRING | The date of the session in YYYYMMDD format. |
| totals | RECORD | This section contains aggregate values across the session. |
| totals.bounces | INTEGER | Total bounces (for convenience). For a bounced session, the value is 1, otherwise it is null. |
| totals.hits | INTEGER | Total number of hits within the session. |
| totals.pageviews | INTEGER | Total number of pageviews within the session. |
| totals.visits | INTEGER | The number of sessions (for convenience). This value is 1 for sessions with interaction events. The value is null if there are no interaction events in the session.|
| totals.transactions | INTEGER | Total number of e-commerce transactions within the session. |
| trafficSource.source | STRING| The source of the traffic source. Could be the name of the search engine, the referring hostname, or a value of the utm_source URL parameter. |
| hits | RECORD | This row and nested fields are populated for any and all types of hits.|
| hits.eCommerceAction | RECORD | This section contains all of the e-commerce hits that occurred during the session. This is a repeated field and has an entry for each hit that was collected.|
| hits.eCommerceAction.action_type | STRING |  The action type. Click through of product lists = 1, Product detail views = 2, Add product(s) to cart = 3, Remove product(s) from cart = 4, Check out = 5, Completed purchase = 6, Refund of purchase = 7, Checkout options = 8, Unknown = 0. |
| hits.product | RECORD | This row and nested fields will be populated for each hit that contains Enhanced Ecommerce PRODUCT data.|
| hits.product.productQuantity | INTEGER | The quantity of the product purchased.|
| hits.product.productRevenue | INTEGER | The revenue of the product, expressed as the value passed to Analytics multiplied by 10^6 (e.g., 2.40 would be given as 2400000).|
| hits.product.productSKU | STRING| Product SKU.|
| hits.product.v2ProductName| STRING| Product Name.|
***
### II. Project Objectives
- Overview of website activity on key metrics: total page views, total visits and transaction
- Bounce rate per traffic source analysis
- Revenue by traffic source analysis
- Funnel analysis
- Revenue analysis
- Product analysis
- Cohort analysis
***
### III. Project Exploration
**1. Disclaim**
- The resulting query after each query just shows a few rows, for further, please access the project link [HERE], for each query, click the query button to see the full version.
- The possible insights extracted after each query come from the initial observation of the query result without context and the business understanding. To make it more clearer, the actionable insight I wrote would need further check I suggest after each query.
- For further business intelligence and decision-making support, the result might come to the dashboard visualization stage using BI tools (Power BI, Tableau,..) and the context understanding of business.
**2. Project instruction**
- **Query 1:** Calculate total visits, pageviews, transactions for Jan, Feb and March 2017 (order by month)
  These are metrics in **Google Analytics, Digital Marketing, and Web Tracking**.

| Metric | **Definition** | **Example** | **What it tells**  |
| --- | --- | --- | --- |
| **Visit (Session)** | - A single user's interaction with the website  within a certain time frame.                                                                                                      - A visit may include multiple page-views and transactions        | A user enters a site, reads 3 pages, exits | - How many users are engaging with your website?                                                        - Are they returning, or is it mostly new traffic?                                                           - Is marketing campaign bringing in visitors? …  |
| **Pageview**  | - A single webpage load or reloaded in the browser                                                            - Page-views help measure content engagement and website traffic | A user views 3 different pages = 3 pageviews | - How engaged are users with your website?                                                         - Which pages are the most popular?        - Do users explore multiple pages or leave after one? …. |
| **Transaction** | - A transaction refers to a succesful or completed order on an E-commerce website | A user places an order on Tiki = 1 transaction | - How many visitors actually convert into buyers?                                                        -Which products are performing the best?                                                                - Are there drop-offs in the checkout process? |

*Query*

```sql
SELECT
      FORMAT_DATETIME('%b %Y',ModifiedDate) AS period,
      Subcategory as name,
      SUM(OrderQty) as sum_item,
      SUM(LineTotal) as sum_value,
      COUNT(SalesOrderID) as order_cnt
FROM `adventureworks2019.Sales.SalesOrderDetail` as sales
INNER JOIN `adventureworks2019.Sales.Product` as product
ON sales.ProductID=product.ProductID
WHERE sales.ModifiedDate >= TIMESTAMP(
  DATE_ADD(
    (SELECT DATE(MAX(ModifiedDate)) FROM `adventureworks2019.Sales.SalesOrderDetail`),
    INTERVAL -12 MONTH))
GROUP BY period, name
ORDER BY period desc, name asc;
```

*Query result (few rows)*
![Screenshot 2025-04-03 at 23 25 50](https://github.com/user-attachments/assets/01bd8fb7-94bd-48af-ac33-38583358ad0a)

*Example of Possible evaluation*

Getting these 3 metrics altogether and navigating their conversion rate, business can evaluate performance (traffic quality, bottlenecks in user journey,..), therefore suggest right decisions to improve business performance. 
1 initial observation we can see on the data result here:

High Visits + Low (to middle) Page-views + Low Transactions (low CVR from Visit to Pageviews; from Visit to Transactions but high visits)

→ Possible Insights: The problem may lies in the landing page and navigation, when customers visit but can not find the product they want

→ Actionable way forward: To confirm the assumption, the people in charge may go check bounce rate and time on page, analyze user flow to see where they click & where they get stuck,..

**Query2: Bounce rate per traffic source in July 2017 (Bounce_rate = num_bounce/total_visit)**

| **Metric** | **Definition** | **Example** | **What it tells** |
| --- | --- | --- | --- |
| **Bounce** | A visitor enters a website and leaves without interacting with any pages or events | A user visits a product page but leaves without clicking anything → **Bounce recorded**. | - A high bounce rate may indicate poor content, bad UX, or irrelevant traffic. |
| **Traffic Source** | The origin of visitors before landing on your site. | - **Organic Search:** Google, Bing searches.                        - **Paid Search:** Google Ads, Facebook Ads.                        - **Direct:** Typing URL directly.  - **Referral:** Clicking a link from another website. | - Helps identify which marketing channels bring in the most traffic. |
| **Bounce Rate per Traffic Source** | % of visitors from a specific source who bounce. | - **Google Organic Traffic:** 50% bounce rate.                   - **Facebook Ads Traffic:** 80% bounce rate.                   - **Email Campaign Traffic:** 30% bounce rate. | - Helps determine which traffic sources bring about high quality source and poor quality source. |

*Query*
```sql
SELECT trafficSource.source,
       SUM(totals.bounces) as total_bounce,
       SUM(totals.visits) as total_visit,
       SUM(totals.bounces)/SUM(totals.visits) AS bounce_rate
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`
WHERE PARSE_DATE('%Y%m%d', date) BETWEEN DATE '2017-07-01' AND DATE '2017-07-31'
GROUP BY trafficSource.source
ORDER BY total_visit DESC;
```

*Query result (few rows)*
![Screenshot 2025-04-03 at 23 41 56](https://github.com/user-attachments/assets/dd099d44-811e-4498-a38e-9355f7178388)

*Example of Possible evaluation*

Note Google Source brings about the highest total_visit and high bounce_rate 

-> Possible insights: SEO mismatch, poor content engagement, slow page load or poor UX,..

-> Actionable way forward: the people in charge might go check queries driving traffic but causing high bounces, improve SEO performance and content structure, check speed issues and fix if needed,..
- **Query 3: Revenue by traffic source by week, by month in June 2017 (order by source, product revenue)**

| **Metric** | **Definition** | **Example** | **What it tells** |
| --- | --- | --- | --- |
| **Revenue** | Total money earned from sales, transactions, or subscriptions. | An e-commerce store sells **500 T-shirts at $20 each**                                  → **Revenue = $10,000**. | - Measures business success and conversion effectiveness. |
| **Traffic Source** | The origin of visitors before landing on your site. | - **Organic Search:** Google, Bing searches.                        - **Paid Search:** Google Ads, Facebook Ads.                        - **Direct:** Typing URL directly.  - **Referral:** Clicking a link from another website. | - Helps identify which marketing channels bring in the most traffic. |
| **Revenue by Traffic Source** | Breakdown of revenue generated from each traffic source. | Google Ads = $30,000 revenue, Organic Search = $25,000 revenue. | - Shows which sources drive the most sales, not just visits. |

*Query*

```sql
SELECT "week"as time_type,
            FORMAT_DATE('%Y%W',PARSE_DATE('%Y%m%d',date)) as time_formatted,
            trafficSource.source as source, 
            SUM(product.productRevenue)/1000000 as productrevenue
     FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`, UNNEST(hits) as hits,UNNEST(hits.product) as product
     WHERE _table_suffix between '0601'and '0630' and product.productRevenue is not null
     GROUP BY time_type,time_formatted,trafficSource.source
UNION ALL
     SELECT "month"as time_type,
            FORMAT_DATE('%Y%m',PARSE_DATE('%Y%m%d',date)) as time_formatted,
            trafficSource.source as source, 
            SUM(product.productRevenue)/1000000 as productrevenue
     FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`, UNNEST(hits) as hits,UNNEST(hits.product) as product
     WHERE _table_suffix between '0601'and '0630' and product.productRevenue is not null
     GROUP BY time_type,time_formatted,trafficSource.source
ORDER BY source,productrevenue;
```

*Result (few rows)* 
![Screenshot 2025-04-03 at 23 43 53](https://github.com/user-attachments/assets/94d45332-644d-4f3c-ba24-3ff38eeac94f)

*Example of Possible evaluation* 

- Direct source has the highest product revenue by source in June 2017.
-> Possible insight: repeated customers who have a high retention rate, campaign success,..

-> Actionable way forward: check the customers who drive revenue source, if they are old users. They might be repeated users with high retention rate who buy the product with routine frequency. 

Otherwise, they might have bookmarked the product before, then returned to buy,.. if new users, they might come from campaign success,..

- **Query 4: Average number of pageviews by purchaser type (purchasers vs non-purchasers) in June, July 2017**

| **Metric** | **Definition** | **Example** | **What it tells** |
| --- | --- | --- | --- |
| **Pageviews (PV)** | - A single webpage load or reloaded in the browser                                                            - Page-views help measure content engagement and website traffic | A user views 3 different pages = 3 pageviews | - How engaged are users with your website?                                                         - Which pages are the most popular?                                - Do users explore multiple pages or leave after one? …. |
| **Purchase Type** | Different categories of buyers based on their behavior. In this dataset, there are 2 purchaser type: purchaser and non-purchaser.  | **New vs. Returning vs. Guest vs. Subscription** buyers. | Returning customers browse less, new customers explore more. |
| **Avg. PV by Purchase Type** | The average number of pages viewed per purchase type. | New customers need **15 PV**, while returning buyers need only **9 PV**. | High CVR PV-PT means users find what they need quickly and buy without hesitation; Low CVR PV-PT means users browse but struggle to decide, leading to drop-offs. |

*Query* 
```sql
with non_purchaser as
   (SELECT 
    FORMAT_DATE('%Y%m', PARSE_DATE('%Y%m%d', date)) AS time_formatted,
    SUM(totals.pageviews) / COUNT(DISTINCT FullVisitorId) AS avg_nonpurchaser
    FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`, 
          UNNEST(hits) AS hits,
          UNNEST(hits.product) AS product
    WHERE _table_suffix BETWEEN '0601' AND '0731' AND totals.transactions is null AND product.productRevenue is null
    GROUP BY time_formatted),
purchaser as 
    (SELECT 
    FORMAT_DATE('%Y%m', PARSE_DATE('%Y%m%d', date)) AS time_formatted,
    SUM(totals.pageviews) / COUNT(DISTINCT FullVisitorId) AS avg_purchaser
    FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`, 
          UNNEST(hits) AS hits,
          UNNEST(hits.product) AS product
    WHERE _table_suffix BETWEEN '0601' AND '0731' 
       AND totals.transactions is not null 
       AND product.productRevenue is not null
    GROUP BY time_formatted)
SELECT purchaser.time_formatted,
       avg_purchaser,
       avg_nonpurchaser
FROM non_purchaser
INNER JOIN purchaser 
ON non_purchaser.time_formatted=purchaser.time_formatted
ORDER BY time_formatted;
```
*Result(few rows)*
![Screenshot 2025-04-03 at 23 46 06](https://github.com/user-attachments/assets/d62155bd-b00c-490c-90d9-6175c2b5c327)

*Example of Possible evaluation* 
- Overall, the average of page-views per non_purchaser type is 3 times from purchaser_type.
-> Possible insight and actionable way forward:

→ For avg_purchaser, we can turn back to the evaluation from page-view to revenue in query 3, the succes of convert may comes from landing page, navigation or SEO,.. to check

→ For avg_non purchaser, we can for example turn back to the problem of bounce rate of query 2 to check the reason behind. 

- **Query 5:  Average number of transactions per user that made a purchase in July 2017**

| **Metric** | **Definition** | **Example** | **What it tells**  |
| --- | --- | --- | --- |
| **Transactions** | The total number of completed purchases. | 1,500 total purchases. | More transactions = higher sales; fewer transactions = possible checkout issues. |
| **Users (Unique Purchasing Users)** | The number of unique individuals who made at least one purchase. | 1,200 unique users made a purchase. | Shows how many distinct customers are buying. |
| **Transactions Per User** | The average number of transactions per user who made a purchase. | **1,500 ÷ 1,200 = 1.25** (each user buys 1.25 times on average). | High value = possible repeat buyers; Low value = possible mostly one-time customers,.. |

*Query* 
```sql
SELECT
     FORMAT_DATE('%Y%m', PARSE_DATE('%Y%m%d', date)) AS month,
     SUM(totals.transactions) / COUNT(DISTINCT FullVisitorId) AS avg_transaction_purchaser
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
     UNNEST(hits) AS hits,
    UNNEST(hits.product) AS product
WHERE _table_suffix between '0701' and '0731'
      AND totals.transactions is not null
      AND product.productRevenue is not null 
GROUP BY month;
```

*Result(few rows)*
![Screenshot 2025-04-03 at 23 51 01](https://github.com/user-attachments/assets/0e23ef53-aaf4-40ac-9b6f-1a0fdc1d2881)

*Example of Possible evaluation* 

A user has an average 4 transactions 

Actionable way forward to explore: 

- Explore purchasers who have transactions based on demographic, age,.. to see the pattern trend and what drive them.
- **Query 6: Average amount of money spent per session. Only include purchaser data in July 2017 (avg=total_revenue/total_session)**

| **Metric** | **Definition** | **Key Insights** | **Example** |
| --- | --- | --- | --- |
| **Revenue** | The total amount of money earned from transactions (purchases). | More revenue means higher sales; low revenue could indicate low conversion rates. | $50,000 total sales. |
| **Session** | A period of user activity on a website (ends after inactivity or timeout). | More sessions mean more visitors, but not necessarily more purchases. | 10,000 sessions in a month. |
| **Average Revenue per Session (only include purchaser data)** | The average amount of money earned per session. | RPV helps businesses understand if their website traffic is profitable, and ensures marketing money is spent efficiently.  | **$50,000 ÷ 10,000 = $5.00** per session. |

*Query*

```sql
SELECT
     FORMAT_DATE('%Y%m', PARSE_DATE('%Y%m%d', date)) AS month,
     SUM(product.productRevenue) / (COUNT(visitId)*1000000) AS avg_moneyspentpersession_purchaser
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
     UNNEST(hits) AS hits, 
     UNNEST(hits.product) AS product
WHERE _table_suffix between '0701' and '0731'
      AND totals.transactions is not null
      AND product.productRevenue is not null 
GROUP BY month
```

*Results(few rows)* 
![Screenshot 2025-04-03 at 23 53 00](https://github.com/user-attachments/assets/5ba61c54-a479-4fea-8892-a8807b754c16)

*Example of Possible evaluation* 

-> Actionable way forward:

- In this case, we should also consider total_revenue, a usual metric benchmark in companies, pattern trend, … and the business objective to make insightful evaluation
- Dig down in high purchaser to evaluate their characteristics
- Segment their source to indicate whether it makes a difference.
- **Query 7:** Other products purchased by customers who purchased the product "YouTube Men's Vintage Henley" in July 2017. Output should show product name and the quantity was ordered.

| **Metric** | **Definition** |
| --- | --- |
| Product name | Product name |
| Order quantity | Quantity purchased per product |

*Query on Bigquery*

```sql
with buyer_of_henley as 
  (SELECT
    FullVisitorId,
    product.v2ProductName as product_name,
    SUM(product.productQuantity) as quantity
    FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*`,
    UNNEST(hits) AS hits, 
    UNNEST(hits.product) AS product
    WHERE _table_suffix between '0701' and '0731'
    AND totals.transactions is not null
    AND product.productRevenue is not null 
    AND product.v2ProductName  = "YouTube Men's Vintage Henley"
    GROUP BY product_name,FullVisitorId )
SELECT product.v2ProductName AS product_name,
       SUM(product.productQuantity) AS total_quantity 
FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*` as full_table,
    UNNEST(hits) AS hits, 
    UNNEST(hits.product) AS product
INNER JOIN buyer_of_henley
ON buyer_of_henley.FullVisitorId = full_table.FullVisitorId
    WHERE  _table_suffix between '0701' and '0731' 
    AND product.v2ProductName <> 'YouTube Men\'s Vintage Henley'
    AND totals.transactions is not null
    AND product.productRevenue is not null 
GROUP BY product_name
ORDER BY total_quantity DESC;
```

*Result(few rows)* 

![Screenshot 2025-04-03 at 23 54 11](https://github.com/user-attachments/assets/723906d9-4b99-40c8-8943-a5aa9353e0b6)

*Example of Possible evaluation* 

- Knowing the product purchased beside one product would help business to define the pattern purchase of customers, especially for products that have dependent products
- **Query 8:**  Calculate cohort map from product view to addtocart to purchase in Jan, Feb and March 2017.

Note: 

hits.eCommerceAction.action_type = '2' is view product page; hits.eCommerceAction.action_type = '3' is add to cart; hits.eCommerceAction.action_type = '6' is purchase

| **Metric** | **Definition** | **Example** | **What it tells?**  |
| --- | --- | --- | --- |
| **Product View**  | When a user visits a product page to see details (price, images, description, etc.). | 10,000 users viewed an iPhone product page. | Measures product interest and helps identify which products attract visitors. |
| **Add to Cart** | When a user adds a product to their shopping cart but hasn’t purchased yet. | 5,000 users added an iPhone to their cart, but only 3,000 completed checkout. | Shows purchase intent—users are interested but may not complete checkout. |
- What is cohort?

**→ Cohort Analysis:** Cohort analysis is a type of behavioral analysis that segments data within a dataset into related groups before performing analysis. 

These groups, or cohorts, typically share common characteristics or experiences within a defined period of time.

Each group of users is a cohort—participants in an experiment across their lifecycles. You can compare cohorts against one another to see if, on the whole, key metrics are getting better over time.

**→ Types of cohorts:**

**Event-Based Cohorts**

This subset of behavioral cohorts groups customers based on a specific event or action—for example, all users who purchased an item during a Black Friday sale.

**Time-Based Cohorts**

This groups customers based on a specific timeframe—for example, all users who downloaded a fitness tracking app in January.

**Size-Based Cohorts.**

This groups customers by size, such as net worth or number of employees—for example, all customers who are small businesses.

**Funnel-Based Cohorts**

This groups customers according to their stage in a funnel—for example, all the people who have put an item in their online shopping cart but have not started the checkout process.
![Screenshot 2025-04-04 at 08 02 30](https://github.com/user-attachments/assets/ae53128e-9d77-4502-b01e-f0e6ffe1b60a)

A cohort analysis presents a much clearer perspective.

Cohort analysis can be done for revenue, churn, viral word of mouth, support costs, or any
other metric business care about.

**→ Type of cohort for this query:** 

This query use funnel-based cohort in 3 month: Jan, Feb and March.

*Query*

```sql
With raw_data as 
    (SELECT
     FORMAT_DATE('%Y%m', PARSE_DATE('%Y%m%d', date)) AS month, 
     SUM (CASE WHEN hits.eCommerceAction.action_type = '2' then 1 else 0 end) as num_product_view,
     SUM (CASE WHEN hits.eCommerceAction.action_type = '3' then 1 else 0 end) as num_addtocart,
     SUM (CASE WHEN hits.eCommerceAction.action_type = '6'and product.productRevenue is not null  then 1 else 0 end) 
    as num_purchase,
     FROM `bigquery-public-data.google_analytics_sample.ga_sessions_2017*` as full_table,
     UNNEST(hits) AS hits, 
     UNNEST(hits.product) AS product
     WHERE _TABLE_SUFFIX BETWEEN '0101' AND '0331'
     GROUP BY month)
SELECT 
      month,
      num_product_view,
      num_addtocart,
      num_purchase,
      ROUND(num_addtocart*100/num_product_view,2) as add_to_cart_rate,
      ROUND(num_purchase*100/num_product_view,2) as purchase_rate
FROM raw_data 
ORDER BY month;
```
*Result(few rows)* 
![Screenshot 2025-04-03 at 23 55 49](https://github.com/user-attachments/assets/c931fc6c-8351-45a2-bfea-1325f049f3be)

*Example of Possible evaluation* 

- We possibly can see the can see the increasing of CVR over time.
-> Actionable way forward: (need insight from business context to make evaluation:

- Analyze drop-off between "Add to Cart" → "Purchase".

- Segment users by traffic source (Google, Facebook, Direct, etc.) to see which source drives the best conversion.

- Check seasonal effects (e.g., promotions in March might explain the higher purchase rate).

- Use A/B testing to test checkout optimizations and improve purchase rates.

**III. Conclusion**

- By exploring of the eCommerce on Google BigQuery through SQL, those reveal several interesting insight for each business questions and prompt actionable ways forward for business optimization. The project prove the power of using SQL on Google Bigquery to gain insights through large datasets.
- From aboved exploration, we explore varied metrics used in Ecommerce such as pageviews, churn rate, total_revenue, conversion rate over stages,.. each of metrics indicates severals insigights, understanding those and their pattern is crucial for tracking and making accurate evaluating.
- To gain comprehensive insight and evaluate key trends for business, next steps would be about using Business Intelligence tools (Power BI, Tableau,..) to build up dashboard and report to business people in charge and suggest actionable ways forward.
