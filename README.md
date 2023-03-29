# RBD---SQL-Server
Montagem de uma tabela de consulta de alguns indicadores através do RDB SQL Server
-- Download Banco de Dados Adventurework 2014

-- Indicadores

-- Toltal de Vendas de Internet por Categoria
-- Receita total de Internet por Mês do pedido
-- Receita e Custo total de Internet por país 
-- Total de vendas de internet por sexo

-- OBS: O ano de análise será apenas 2013 (ano do pedido)

SELECT * FROM FactInternetSales
SELECT * FROM DimProductCategory
SELECT * FROM DimSalesTerritory
SELECT * FROM DimCustomer

-- Aqui precisaremos fazer um relacionamento em cadeia

CREATE OR ALTER VIEW VENDAS_INTERNET AS
SELECT 
	fis.SalesOrderNumber AS 'No PEDIDO',
	fis.OrderDate AS 'DATA PEDIDO',
	dpc.EnglishProductCategoryName  AS 'CATEGORIA PRODUTO',
	dc.FirstName + ' ' + Lastname AS 'NOME CLIENTE',
	SalesTerritoryCountry AS 'PAÍS',
	fis.OrderQuantity AS 'QUANTIDADE VENDIDA',
	fis.TotalProductCost AS 'CUSTO VENDA',
	fis.SalesAmount AS 'RECEITA VENDA'

FROM FactInternetSales fis
INNER JOIN DimProduct dp ON fis.ProductKey = dp.ProductKey
	INNER JOIN DimProductSubcategory dps ON dp.ProductSubcategoryKey = dps.ProductSubcategoryKey
		INNER JOIN DimProductCategory dpc ON dps.ProductSubcategoryKey = dpc.ProductCategoryKey
INNER JOIN DimCustomer dc ON fis.CustomerKey = dc.CustomerKey
INNER JOIN DimSalesTerritory dst ON fis.SalesTerritoryKey = dst.SalesTerritoryKey

WHERE YEAR(OrderDate) = 2013
