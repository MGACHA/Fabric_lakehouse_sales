SELECT
    Region,
    SUM(Sales) as total_sales
FROM clean_sales
GROUP BY Region
ORDER BY total_sales DESC;


SELECT DP.Category, Count(DP.Product_ID) as Cat from fact_sales as FS
INNER JOIN dim_product as DP
on DP.Product_ID = FS.Product_ID
Group by DP.Category

SELECT DP.Category, Count(DP.Product_ID) as Cat from fact_sales as FS
INNER JOIN dim_product as DP
on DP.Product_ID = FS.Product_ID
where DP.Product_ID = 'OFF-PA-10000477'
Group by DP.Category


SELECT * from dim_product as DP
where DP.Product_ID = 'OFF-PA-10000477'

SELECT * from dim_product as DP
where Product_Name = 'Xerox 22'

SELECT * from dim_product as DP
where Product_Name = 'Xerox 1952'

SELECT * from fact_sales


