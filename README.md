# AdventureWorksDW2020---Power-BI
## Datasource:
  * AdventureWorksDW2020
    * FactInternetSales
    * DimProduct
    * DimSub-Product
    * DimCategory
    * DimDate
    * DimGeography
    * DimSalesTeritory
* Live Dashboard : https://s.id/1ZbP0

## EDA
1. Total Sales 2020
   
  `Select sum(SalesAmount) total_sales from FactInternetSales
  where OrderDate between '2020-01-01' and '2020-12-31'`

2. Total Profit 2020

`select sum(SalesAmount)-sum(TotalProductCost)-(sum(TaxAmt)+sum(Freight)) as profit
from FactInternetSales
where OrderDate between '2020-01-01' and '2020-12-31'`
 
4. Total Product Sold 2020

`select sum(OrderQuantity) from FactInternetSales
where OrderDate between '2020-01-01' and '2020-12-31'`
 
5. Total Sales by Date in 2020

`select distinct(format(b.OrderDate, 'yyyy-MM')) as Date, sum(b.SalesAmount) as Total_Sales
from DimDate a join FactInternetSales b on a.FullDateAlternateKey = b.OrderDate
where b.OrderDate between '2020-01-01' and '2020-12-31'
group by format(b.OrderDate, 'yyyy-MM')
order by Date` 
  
7. Total Profit by Continent in 2020

`select t.SalesTerritoryGroup as Continent ,t.SalesTerritoryCountry,
(sum(SalesAmount)-sum(TotalProductCost)-(sum(TaxAmt)+sum(Freight))) as profit
from FactInternetSales s join DimSalesTerritory t on s.SalesTerritoryKey = t.SalesTerritoryKey
where s.OrderDate between '2020-01-01' and '2020-12-31'
group by t.SalesTerritoryGroup, t.SalesTerritoryCountry
order by 3 desc`
 
7. Total Profit by Category in 2020

`select pc.EnglishProductCategoryName as Category ,
(sum(SalesAmount)-sum(TotalProductCost)-(sum(TaxAmt)+sum(Freight))) as profit
from FactInternetSales s join DimProduct p on s.ProductKey = p.ProductKey
join DimProductSubcategory ps on p.ProductSubcategoryKey = ps.ProductSubcategoryKey
join DimProductCategory pc on ps.ProductCategoryKey = pc.ProductCategoryKey
where s.OrderDate between '2020-01-01' and '2020-12-31'
group by pc.EnglishProductCategoryName
order by 2 desc`

8. Total Quantity Sold by Sub-Category in 2020

`select ps.EnglishProductSubcategoryName as Category ,
sum(s.OrderQuantity) product_sold
from FactInternetSales s join DimProduct p on s.ProductKey = p.ProductKey
join DimProductSubcategory ps on p.ProductSubcategoryKey = ps.ProductSubcategoryKey
join DimProductCategory pc on ps.ProductCategoryKey = pc.ProductCategoryKey
where s.OrderDate between '2020-01-01' and '2020-12-31'
group by ps.EnglishProductSubcategoryName
order by 2 desc`

 
9. Top 5 Sales by Customer in 2020

`select top 5 concat(b.FirstName,' ',b.LastName) as Customer_name,
sum(a.SalesAmount) as total_sales,
(sum(SalesAmount)-sum(TotalProductCost)-(sum(TaxAmt)+sum(Freight))) as profit
from FactInternetSales a join DimCustomer b on a.CustomerKey = b.CustomerKey
where a.OrderDate between '2020-01-01' and '2020-12-31'
group by concat(b.FirstName,' ',b.LastName)
order by 2 desc`
