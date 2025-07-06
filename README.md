# KULTRA_MEGA-STORES-INVENTORY-ANALYSIS

## PROJECT OVERVIEW

This project focuses on analysing sales amd customer data from 2009 to 2012 and presenting key insights and findings.




## OBJECTIVE

 * Identify product category with the highest sales
 * Find the Top 3 and Bottom 3 regions in terms of sales
 * Analysis the total sales of appliances in Ontario
 * Advise the management of KMS on what to do to increase the revenue from the bottom 
10 customers
* Examine order priority versus shipping method
* Business customer with the highest sales
* Corporate Customer placed with the most number of orders in 2009 – 2012
* Track customer that returned items, and segment they belong to
* The most valuable customers, and what products they purchased


## TOOLS USED
* SQL mangenent studio
* MsSQL - For creating Database
  - Data cleaning which include,
  - Altering table and columnn
  - For querying data using syntax like, Group By, Order BY and Where.
  - Aggregating (SUM, COUNT, AVERAGE)
  - Foe joining tables


## KEY INSIGHTS


* **Technology products** have the highest average revenue per order — consider upselling strategies.
* Shipping cost was spent based on order priority as estimated shipping cost was was rated with the order count, Regular air acccrued the most cost.
* Customers that returned items where from the corporate, small business and home office segment.
* Themost valuable customers often buy products from the 'Technology' and 'Furniture' category



## QUERY AND VISUALS


--QUESTION 1--------TOP SELLING CATEGORY---------
```SQL
  SELECT PRODUCT_CATEGORY, SUM(SALES) AS TOTALSALES
FROM [KMS DATA]
GROUP BY PRODUCT_CATEGORY
ORDER BY TOTALSALES DESC;
````
