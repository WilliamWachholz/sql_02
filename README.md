# sql_02

ALTER TABLE Produto ADD ValorCompra DECIMAL(10,2), ValorVenda DECIMAL(10,2)

SELECT * FROM Produto

UPDATE Produto SET ValorCompra = 1.5

UPDATE Produto SET ValorVenda = 2 WHERE ID = 1

UPDATE Produto SET ValorVenda = 0 WHERE ID <> 1

SELECT  *, 
		ValorCompra + ValorVenda AS Soma
FROM Produto
