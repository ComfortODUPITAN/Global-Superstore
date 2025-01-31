# GLOBAL SUPERSTORE

## Project Overview
---
Global Superstore is a global online retailer based in New York, boasting a broad product catalogue and aiming to be a one-stop-shop for its customers. The superstore’s clientele, hailing from 147 different countries, can browse through an endless offering with more than 10,000 products. This large selection comprises three main categories: office supplies (e.g., staples), furniture (e.g., chairs), and technology (e.g., smartphones)

## Objective
---
- To draw out meaningful insight from the Superstore dataset which would aid management in making informed decisions to improve performance and profitability
- To identify the three countries that generated the highest total profit for Global Superstore in 2014
- To find the three products with the highest total profit for each of these three countries
- Identify the 3 subcategories with the highest average shipping cost in the United States
- To know the factors that might be responsible for Nigeria’s poor performance
- To Identify the product subcategory that is the least profitable in Southeast Asia
- To identify the city with the least profit in the United States
- To know why this city’s average profit so low?
- To show the product subcategory with the highest average profit in Australia
- To identify the most valuable customers and what they purchased
  
## Tool
- Microsoft Excel
- PowerBI
- SQL
  
## Methodology
---
### 1. Data Source
[Data collection](https://docs.google.com/spreadsheets/d/1aXLn5bewi_D2jsCmEdMXfnU9tkjoZmel/edit?usp=sharing&ouid=104059753206145262133&rtpof=true&sd=true)
### 2. Data Cleaning
#### Microsoft Excel- to clean the dataset

![Unclean dataset](https://github.com/user-attachments/assets/442d0da5-f954-45cf-94aa-9093291def96)

- Remove duplicates
- Checked for inconsistent values
- Deleted empty rows and columns
- Fill in the empty cells
- Checked for anomalies

![Cleaned dataset](https://github.com/user-attachments/assets/e752b850-a224-4e40-9f4f-257fea612208)

### 3. Analysis and Insights
#### Analysis
#### SQL- to analyze the dataset
[Download Postgresql](https://www.postgresql.org/download/)
- By filtering and manipulating the datasets to get the objectives of the project
```sql
CREATE TABLE globalstores_order AS
SELECT order
FROM globalstores AS g
LEFT JOIN return_table AS r
ON g.order_id=r.order_id

SELECT COUNT (DISTINCT country) FROM globalstores_order;

SELECT distinct country, SUM (profit) AS total_profit
FROM globalstores_order
WHERE order_date BETWEEN '2014-01-01' AND '2014-12-31'
GROUP BY country
ORDER BY total_profit DESC;

SELECT country, product_name,SUM (profit)::numeric (10,2)  AS total_profit
FROM globalstores_order
WHERE country = 'United States'
AND  order_date BETWEEN '2014-01-01' AND '2014-12-31'
GROUP BY country,product_name
ORDER BY total_profit DESC;

SELECT country,sub_category,AVG(shipping_cost)::numeric(10,2) AS "average shipping cost"
FROM globalstores_order
WHERE country = 'United States'
GROUP BY country,sub_category
ORDER BY "average shipping cost" DESC;

SELECT country,region,AVG (shipping_cost)::numeric (10,2) AS "shipping cost", AVG (discount)::numeric (10,2) AS "average discount", 
SUM (profit) AS "total profit"
FROM globalstores_order
WHERE region = 'Africa'
AND  order_date BETWEEN '2014-01-01' AND '2014-12-31'
GROUP BY  country,region
ORDER BY "total profit", "average discount", "shipping cost";

SELECT sub_category,region,SUM (profit) AS "total profit"
FROM globalstores_order
WHERE region = 'Southeast Asia'
GROUP BY  sub_category,region
ORDER BY "total profit" ASC;

SELECT city,quantity,country,AVG (profit)::numeric (10,2) AS "average profit"
FROM globalstores_order
WHERE country = 'United States' 
AND NOT quantity < 10
GROUP BY city,quantity,country
ORDER BY "average profit" ASC;

SELECT city,quantity,shipping_cost,country,AVG (profit)::numeric (10,2) AS "average profit"
FROM globalstores_order
WHERE country = 'United States' 
AND NOT quantity < 10
GROUP BY city,quantity,country,shipping_cost
ORDER BY "average profit" ASC;

SELECT sub_category,country,AVG (profit)::numeric (10,2) AS "average profit"
FROM globalstores_order
WHERE country = 'Australia'
GROUP BY  sub_category,country
ORDER BY "average profit" DESC;

SELECT customer_name,sub_category,SUM(profit)::numeric(10,2) AS "total profit"
FROM globalstores_order
GROUP BY customer_name,sub_category
ORDER by "total profit" desc;
```

#### Insights
![INSIGHTS](https://github.com/user-attachments/assets/700be62e-ae20-4df8-a10f-3d01a367aea9)

![INSIGHTS 2](https://github.com/user-attachments/assets/14955aae-558f-4532-9206-0d2f05f14c51)

### 4. Visualization
#### Power BI- for visualizing the dataset
[Download PowerBI](Microsoft.com)

![DASHBOARD 1](https://github.com/user-attachments/assets/e05050d0-75e6-4963-9922-36491fdf9274)

![DASHBOARD 2](https://github.com/user-attachments/assets/27861212-3714-4ac6-bfa2-ef8d701894b2)

### 5. Interpretation 

- The three countries that generated the highest profit for global superstore in 2014 are UNITED STATES with a total pofit of 93484.7249, INDIA 
  with a total profit of 48807.675, CHINA with a total profit of 46793.994
- In United States, the three products with the highest total profit are Canon imageCLASS 2200 Advanced Copier, Hewlett Packard LaserJet 3310 
  Copier,GBC DocuBind TL300 Electric Binding System with total profit of 15679.96,3623.94,1910.59 respectively.

  In India, the three products with the highest total profit are Sauder Classic Bookcase Traditional,Cisco Smart Phone with Caller ID, Hamilton 
  Beach Refrigerator Red with total profit of 2419.65,1609.38,1440.24 respectively.

  In China, the three products with the highest total profit are Sauder Classic Bookcase Metal, Bush Classic Bookcase Mobile, HP Copy Machine 
  Color with a total profit of 1463.07,1220.52,1196.13 respectively
- In the United States, copiers,machines and tables are the top 3 sub-categories with average shipping cost of 165.29,132.25,69.95
- One of the factors responsible for poor performance in Nigeria is the high shipping cost,considering its a country that relies on importing and 
  exporting of goods.Insufficient supply chains as well leads to a higher shipping cost. Nigeria has smaller economies which results in the 
  reduction of shipping costs and lower percent of discount. High trade barrier and tariff also affect the shipping cost negatively as it hinders 
  trade and economic growth in the country
- Table in the product sub-category has a total profit of -18618.3051 which makes it the least profitable in Southeast Asia
- The least profitable city in the United States is Concord with an average profit of -1862.31 and does not have less than 10 orders
- The city has a high shipping cost which makes it depend heavily on imported goods for consumption or prodution inputs, the high shipping cost 
  also restricts market access for businesses in the city causing difficulty in reaching other customers in other location;this contributes to 
  lower average profits with the high shipping cost in the city, industries that rely on exporting goods from the city face hindrances in their 
  profitability. with the high shipping cost as well,the city struggles to compete with businesses located in areas with lower transportation fee
- With an average profit of 139.01, APPLIANCES turn out to be the sub-category with the highest average profit from the product sub-category in 
  Australia
- The most valuable customers are;Tamara Chand,Raymond Buch,Sanjit Chand and they purchased Copiers

## Recommendations
---
- Given that the superstore has a profit of $93,484, the store should focus on enhancing the customer experience both online and instore.
- The introduction of new product line should be made to capture additional market share
- Foster a culture of continuous improvement and innovation within the business organization and adapt strategies to evolving market conditions to 
  stay competitive and profitable
- Shipping cost has been identified to be one of the reasons for low profit in some of the countries under global superstore, consolidate 
  shipments, negotiate volume discounts and explore alternative shipping options. Use lightweight and compact packaging materials that provide 
  adequate protection for products while reducing shipping cost
- Implement cost reduction measures to streamline expenses and improve profit margins
- Explore strategic partnerships with complementary businesses to unlock new growth opportunity
- The root cause behind the high amount in the returned item from the segment should be known. Evaluating factors such as product quality, 
  misleading product description for the customers, will help in rectifying the cause of returned items.
- Actively listen to the customers and use it to identify areas for improvement
- Provide comprehensive and accurate product information to customers, this includes the features, benefits, and limitations of the product to 
  help them make informed purchasing decisions.
- Considering the customers that purchase more from global superstore, a dedicated customer service representative should be assigned to provide 
  proactive assistance and address any concerns promptly.
- Offer membership benefits to show appreciation for their patronage
- Provide assistance with product selection and offer complimentary gift wrapping 
- Recommend complementary products or upgrades based on the customer's past purchases and preferences
- Maintain ongoing communication and engagement with the customers to nurture the relationship over time
- The store can strengthen its relationship with high value customers, increase customer retention, and drive long term profitability by 
  implementing these recommendations.








