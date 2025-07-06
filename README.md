# DSA-PROJECT-ON-SQL

## project overview 

## Data Cleaning Process

## Project Analysis

## Question 1: Which product category had the highest sales?


          select top 1 Product_Category,
          sum(sales) As total_sales
          from  [dbo].KMSORDER
          GROUP BY Product_Category
          ORDER BY total_sales DESC

ANSWER: Product Category = (Technology) --- Sales = $605,426.02

![Answer1](https://github.com/user-attachments/assets/19bbdabb-8fc7-4947-8658-0434103a6fda)


## Question 2 What are the top 3 and bottom 3 regions in terms of sales

####Top 3

Select top 3 Region,
sum(sales) As Total_sales
from KMSORDER
group by region
order by total_sales desc

ANSWER: Top 3 Regions; Ontario = $471,161.61 / West = $375,122.39 / Prarie = $296,732.24 


    ![Answer 3](https://github.com/user-attachments/assets/fe222804-088d-46c2-983e-5a1470d6137b)

### Bottom 3

SELECT top 3 Region,
MIN(SALES) AS Total_sales
from [dbo].KMSORDER
group by region
order by total_sales Asc

ANSWER: B0ttom 3 Regions; Ontario = $4.94, West = $5.06, Prarie $5.63


![Answer 2 bottom 3 region](https://github.com/user-attachments/assets/149d655e-988b-4ed3-bd46-45cebf4f8eff)




### Question 3: What were the total sales of appliances in Ontario?


SELECT Region,
    SUM(Sales) AS Total_Sales
FROM [dbo].KMSORDER
WHERE Region = 'Ontario'
  And Product_Sub_Category = 'appliances'
Group By Region

ANSWER: = Total Sales of Appliances in Ontario = $17,648.37


![Answer 3](https://github.com/user-attachments/assets/ce016fe4-647c-440f-9b05-3e6173612a42)



### Question 4: Advise the management of KMS on what to do to increase the revenue from the bottom 10 customer.


Select Top 10
      Customer_Name,
	  Sum(Sales) As Total_Revenue
From [dbo].[KMSORDER]
Group by Customer_Name
Order by Total_Revenue Asc

BOTTOM 10 CUSTOMERS
--Customer Name          Total Sales

--John Grady       =     $5.06

--Frank Atkinson   =     $10.48

--Sean Wendt       =     $12.80

--Sandra Glassco   =     $16.24

--Katherine Hughes =     $17.77

--Bobby Elias      =     $22.56

--Noel Staavos     =     $24.91

--Thomas Boland    =     $28.01

--Brad Eason       =     $35.17

--Theresa Swint    =     $38.51

ANSWER: MY ADVISE TO THE MANAGEMENT OF KMS IN ORDER TO INCREASE THE REVENUE FROM THE BOTTOM 10 CUSTOMERS

 a- Identify the bottom 10 customers by total sales.
 
b- Analyze purchase patterns and feedback.

c- Offer targeted promotions or loyalty incentives.

d- Assign dedicated account managers or support for better engagement.

e- Upsell or cross-sell complementary products.

f- Improve delivery timelines and customer service


![Answer 4a](https://github.com/user-attachments/assets/26fff7d4-c952-48d9-a827-b443bd6313f5)


![Answer 4b advise to management](https://github.com/user-attachments/assets/2afb7711-37f4-4cfe-8b73-779d4e4d3548)



### Question 5: KMS incurred the most shipping cost using which shipping method?

Select Top 1
      Order_Priority, Ship_Mode,
	  Sum(Shipping_Cost) As Total_Cost
From [dbo].KMSORDER
Group by Order_Priority, Ship_Mode
Order by Total_Cost Desc

ANSWER: Order_Priority = (Low) / Shiping Method = Delevery Struck / Shiping Cost = $1,659.14


![Answer 5](https://github.com/user-attachments/assets/abfa1ff5-de6e-4fe5-a62d-ab7dadb5a5ea)




### Question 6: Who are the most valuable customers, and what products or services do they typically purchase?

WITH CustomerRevenue AS (
    SELECT
        Customer_Name,
        Customer_Segment,
        SUM(Sales) AS Total_Revenue
    FROM [dbo].KMSORDER
    GROUP BY Customer_Name, Customer_Segment
),
CustomerProductRevenue AS (
    SELECT 
        Customer_Name,
        Product_Name,
		Order_Quantity,
        SUM(Sales) AS Product_Revenue,
		ROW_NUMBER() OVER (
            PARTITION BY Customer_Name 
            ORDER BY SUM(Sales) DESC, Product_Name DESC
        ) AS Product_Rank
    FROM [dbo].KMSORDER
    GROUP BY Customer_Name, Product_Name, Order_Quantity
	)
SELECT TOP 5
    cr.Customer_Name,
    cr.Customer_Segment,
    cpr.Product_Name AS Top_Product,
	cpr.Order_Quantity,
    cr.Total_Revenue
FROM CustomerRevenue cr
JOIN CustomerProductRevenue cpr 
    ON cr.Customer_Name = cpr.Customer_Name 
    AND cpr.Product_Rank = 1
ORDER BY cr.Total_Revenue DESC

ANSWER: We have our Top 5, having that the (top product that are being purchased are as follow:

Customer_Name       Customer_Segment     Top_Product                                                                  Total_Revenue                         

1-John Lucas          Small Business      Chromcraft Bull-Nose Wood 48" x 96" Rectangular Conference Tables             $37,919.43

2-Lycoris Saunders    Corporate           Bretford CR8500 Series Meeting Room Furniture                                 $30,948.18

3-Grant Carroll       Small Business      Fellowes PB500 Electric Punch Plastic Comb Binding Machine with Manual Bind   $26,485.12

4-Erin Creighton      Corporate           Polycom VoiceStation 100                                                      $26,443.02

5-Peter Fuller        Corporate           Panasonic KX-P3626 Dot Matrix Printer                                         $26,382.21



![Answer 6a](https://github.com/user-attachments/assets/7324025b-3daf-4b9d-973f-444427553275)




![Answer 6b](https://github.com/user-attachments/assets/1b123cf8-2292-4a4b-9489-202877de70e0)




![Answer 6c top 5](https://github.com/user-attachments/assets/849f5c25-c97c-4ec0-80d1-52a467b7d1a6)




### Question 7: Which (Small business customer) had the highest sales?

WITH CustomerRevenue AS (
    SELECT 
        Customer_Name,
        Customer_Segment,
        SUM(Sales) AS Total_Revenue
    FROM [dbo].KMSORDER
	Where Customer_Segment = 'Small Business'
    GROUP BY Customer_Name, Customer_Segment
),
CustomerProductRevenue AS (
    SELECT 
        Customer_Name,
        Product_Name,
		Order_Quantity,
		SUM(Sales) AS Product_Revenue,
        ROW_NUMBER() OVER (
            PARTITION BY Customer_Name 
            ORDER BY SUM(Sales) DESC, Product_Name DESC
        ) AS Product_Rank
    FROM [dbo].KMSORDER
    GROUP BY Customer_Name, Product_Name, Order_Quantity
)
SELECT TOP 1
    cr.Customer_Name,
    cr.Customer_Segment,
    cpr.Product_Name AS Top_Product,
	cpr.Order_Quantity,
    cr.Total_Revenue
FROM CustomerRevenue cr
JOIN CustomerProductRevenue cpr 
    ON cr.Customer_Name = cpr.Customer_Name 
    AND cpr.Product_Rank = 1
ORDER BY cr.Total_Revenue DESC

ANSWER: The Custormer_Segment = (Small Business) / Customer_Name = (John Lucas) / Total_Revenue = $37,919.43



![Answer 7a](https://github.com/user-attachments/assets/366ea2bf-ab26-470f-a262-de277318c4b2)



![Answer 7b](https://github.com/user-attachments/assets/297aa2dd-6bcb-422b-9271-b57ed9a53b5b)



![Answer 7c](https://github.com/user-attachments/assets/56147e1a-30e4-4a8f-99bf-ebdffe80f75a)



### Question 8: Which (Corporate Customer) placed the most (number of orders) in (2009 – 2012)

SELECT TOP 10
   Customer_Segment, Customer_Name,
    COUNT(Order_ID) AS Total_Orders
FROM [dbo].KMSORDER
WHERE Customer_Segment = 'Corporate'
    AND Ship_Date BETWEEN '2009-01-01' AND '2012-12-31'
GROUP BY Customer_Segment, Customer_Name
ORDER BY Total_Orders DESC


ANSWER: The corporate customer with the most placed order in 2009 - 2012 Is; Customer_Name = Erin Creighton



![Answser 8](https://github.com/user-attachments/assets/0c2f9e79-0210-412a-b92b-024580c2ec6c)


### Question 9: Which (Consumer customer) was the (most profitable one)?

SELECT TOP 1
    Customer_Segment, Customer_Name,
    SUM(Sales) AS Total_Revenue
FROM [dbo].KMSORDER
WHERE Customer_Segment = 'Consumer'
GROUP BY Customer_Segment, Customer_Name
ORDER BY Total_Revenue Desc

ANSWER: The most profitable (Consumer Customer); Customer_Name = Darren Budd / Profitability = $23,034.35


![Answer 9](https://github.com/user-attachments/assets/038ebf62-ca98-4c54-bf92-e8a4465762f1)

### Question 10: Which customer returned items? And what segment do they belong to?

SELECT TOP 572
   Customer_Name, Customer_Segment,
    max([Status]) AS Total_Returned_Items
FROM [dbo].KMSORDER
GROUP BY Customer_Segment, Customer_Name, [Status]
ORDER BY Total_Returned_Items Desc

ANSWER: All customers returned the items


![Answer 10](https://github.com/user-attachments/assets/1444cbe1-a11a-46de-bf52-cbc9bbd118f1)



### Question 11....

If the delivery truck is the most economical but the slowest shipping method and 
Express Air is the fastest but the most expensive one, do you think the company 
appropriately spent shipping costs based on the Order Priority? (Explain your answer)

SELECT  
    Order_Priority,  
    Ship_Mode,  
    SUM(Shipping_Cost) AS Total_Cost  
FROM [dbo].KMSORDER 
WHERE Ship_Mode IN ('Express Air', 'Delivery Truck')  
GROUP BY Order_Priority, Ship_Mode  
ORDER BY Ship_Mode, Total_Cost DESC

ANSWER: Based on Order Priority				Ship Mode					Total Cost

		Low									Delivery Truck				$1,659.14
		High								Delivery Truck				$1,274.98

		Not	Specified						Delivery Truck				$1,040.76

		Medium								Delivery Truck				$925.25

		Critical							Delivery Truck				$829.01

		Low									Express Air					$289.95

	Not	Specified						Express Air					$257.40

	Critical							Express Air					$195.19

	Medium								Express Air					$167.27

High								Express Air					$108.92

Was shipping cost appropriately spent based on Order Priority?

a- Appropriate spending depends on matching high-priority orders with fast (Express Air) shipping and low-priority orders with economical (Delivery Truck) methods.

b- If there are mismatches (e.g., urgent items shipped via Delivery Truck), the spending is inappropriate.

c- A proper audit of the data would confirm alignment.



![Answer 11](https://github.com/user-attachments/assets/9038895a-f6a4-4ec6-8211-be915ce20aba)
















