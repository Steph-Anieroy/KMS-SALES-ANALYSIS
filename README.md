# KULTRA_MEGA-STORES-INVENTORY-ANALYSIS


## FEATURES

[PROJECT_OVERVIEW](#PROJECT_OVERVIEW)

[OBJECTIVE](#OBJECTIVE)

[TOOLS_USED](#TOOLS_USED)

[KEY_INSIGHTS](#KEY_INSIGHTS)

[QUERY_AND_VISUALS](#QUERY_AND_VISUALS)




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


![TOPSELLINGCATEGORY](https://github.com/user-attachments/assets/49098c3d-10a6-4614-8e72-0df4c0ea051e)



### TOP 3 REGIOMS


````SQL
  SELECT TOP 3 REGION, SUM(SALES) AS TOTALSALES
FROM [KMS DATA]
GROUP BY REGION
ORDER BY TOTALSALES DESC;
````

```SQL
SELECT REGION, SUM(SALES) AS TOTALSALES
FROM [KMS DATA]
GROUP BY REGION
ORDER BY TOTALSALES ASC
````


![TOP 3 REGIONS](https://github.com/user-attachments/assets/d5ae5af6-8e1a-4360-b490-d78949877ec7)


### TOTAL SALES OF APPLIANCE IN ONTARIO

````SQL
SELECT REGION, SUM(SALES) AS TOTAL_APPLIANCES_SALES
FROM [KMS DATA]
WHERE PRODUCT_SUB_CATEGORY = 'APPLIANCES'
	GROUP BY REGION
	ORDER BY TOTAL_APPLIANCES_SALES DESC
````


![TOTALSALESOFAPPLIANCESINONTARIO](https://github.com/user-attachments/assets/749a1beb-0758-4a61-9b31-9cbc18ecfaca)


### ADVISE ON HOW TO INCREASE REVENUE FROM BOTTOM 10 CUSTOMERS

````SQL
SELECT 
    CUSTOMER_NAME,
    COUNT(DISTINCT ORDER_QUANTITY) AS TOTAL_ORDER,
    AVG(Sales) AS AVG_ORDER_VALUE,
    MAX(ORDER_PRIORITY) AS MAX_PRIORITY,
    CUSTOMER_SEGMENT,
    REGION
FROM 
    [KMS DATA]
WHERE 
    CUSTOMER_NAME IN (
        SELECT TOP 10 CUSTOMER_NAME
        FROM [KMS DATA]
        GROUP BY CUSTOMER_NAME
        ORDER BY SUM(SALES) ASC
    )
GROUP BY 
    CUSTOMER_NAME, CUSTOMER_SEGMENT, REGION
ORDER BY AVG_ORDER_VALUE ASC
````
````SQL
----------USING TOTAL REVENUE---------

SELECT TOP 10 
    ORDER_QUANTITY,
    CUSTOMER_NAME,
    SUM(SALES) AS TOTAL_REVENUE
FROM 
    [KMS DATA]
GROUP BY 
    ORDER_QUANTITY, CUSTOMER_NAME
ORDER BY 
    TOTAL_REVENUE ASC;
````


![ADVISEONHOWTOINCREASECUTOMERSSALESINONTARIO](https://github.com/user-attachments/assets/668495fd-f8a0-4faa-bb18-039a87474cce)


### -KMS INCURRED THE MOST SHIPPING COST USING WHAT SHIPPING METHOD?

````SQL
SELECT SHIP_MODE, SUM(SHIPPING_COST) AS TOTAL_SHIPPING_COST
FROM [KMS DATA]
GROUP BY SHIP_MODE
ORDER BY (TOTAL_SHIPPING_COST) DESC

SELECT *
FROM [KMS DATA]
WHERE SHIPPING_COST = (
SELECT MAX(SHIPPING_COST)
FROM [KMS DATA]
)
````


![MOST SHIPPIING METHOD](https://github.com/user-attachments/assets/e6db9b02-e6e1-4518-be18-b1ed7cc5bfcd)


#### MOST VALUABLE CUSTOMER AND PRODUCT THEY PURCHASE

````SQL
SELECT TOP 5 CUSTOMER_NAME AS MOST_VALUABLE_CUSTOMER, 
	SALES AS TOTAL_SALES, ORDER_QUANTITY, 
	UNIT_PRICE, PRODUCT_NAME, 
	PRODUCT_CATEGORY, 
	PRODUCT_SUB_CATEGORY
FROM [KMS DATA]
ORDER BY ORDER_QUANTITY DESC
````


#### SMALL BUSINESS CUSTOMER WITH HIGHEST SALES

````SQL
SELECT TOP 1 
   SALES AS HIGHESTSALES, CUSTOMER_SEGMENT
FROM [KMS DATA]
WHERE CUSTOMER_SEGMENT = 'Small Business'
ORDER BY SALES DESC
````

![MOST VALUABEL CUSTOMERS](https://github.com/user-attachments/assets/067257b1-283d-4bf3-9469-8c5c25269f12)

#### CORPORATE CUSTOMER WITH THE MOST NUMBER OF ORDERS FROM 2009 TO 2012

````SQL
SELECT CUSTOMER_NAME, ORDER_QUANTITY AS HIGHEST_ORDER, CUSTOMER_SEGMENT, ORDER_DATE
	FROM [KMS DATA]
	WHERE 
	CUSTOMER_SEGMENT = 'CORPORATE'
	AND
	YEAR(ORDER_DATE) BETWEEN 2009 AND 2012
	ORDER BY ORDER_QUANTITY DESC

	SELECT TOP 1 CUSTOMER_NAME, ORDER_QUANTITY AS HIGHEST_ORDER, CUSTOMER_SEGMENT, ORDER_DATE
	FROM [KMS DATA]
	WHERE 
	CUSTOMER_SEGMENT = 'CORPORATE'
	AND
	YEAR(ORDER_DATE) >=2009 AND  YEAR(ORDER_DATE) <=2012
	ORDER BY ORDER_QUANTITY DESC
````

![CORPORATECUSTOMERWITHHIGHESTORDER](https://github.com/user-attachments/assets/7781503d-53d8-42f8-a68a-ce81ff5233e1)

#### MOST PROFITABLE CONSUMER CUSTOMER

`````SQL
SELECT TOP 1 CUSTOMER_NAME, CUSTOMER_SEGMENT AS MOST_PROFITABLE_CONSUMER, PROFIT
FROM [KMS DATA]
WHERE CUSTOMER_SEGMENT = 'CONSUMER'
ORDER BY PROFIT DESC
``````

![MOSTPROFITABLECONSUMERCUSTOMER](https://github.com/user-attachments/assets/15dabae5-d3a5-4424-9e8c-34d99b44a794)


#### CUSTOMERS THAT RETURNED ITEMS AND THEIR SEGMENT
`````SQL
SELECT [KMS DATA].CUSTOMER_NAME, [KMS DATA].CUSTOMER_SEGMENT, ORDER_STATUS.[STATUS]
FROM [KMS DATA]
INNER JOIN ORDER_STATUS ON [KMS DATA].ORDER_ID =ORDER_STATUS.[ORDER_ID]
WHERE [STATUS] = 'RETURNED'
`````
![CUSTOMERTHATRETURNEDITEMS](https://github.com/user-attachments/assets/a727ffdf-c900-4f6f-b632-e70352cd7c0f)


#### SHIPPPING COST BASED ON ORDER PRIORITY
`````SQL
SELECT 
    ORDER_PRIORITY,
    SHIP_MODE,
    COUNT (*) ORDER_ID,
    AVG(SHIPPING_COST) AS AVERAGE_SHIPPING_COST
FROM 
    [KMS DATA]
GROUP BY 
    ORDER_PRIORITY, SHIP_MODE
ORDER BY 
    ORDER_ID DESC;
`````


<img width="468" height="309" alt="SHIPPING COST BASED ON ORDER PRIORITY" src="https://github.com/user-attachments/assets/aca937f5-0722-4432-af25-7724daeccd13" />


## LICENCE
MIT - For educational purpose.


