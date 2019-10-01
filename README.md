# sql_02

ALTER TABLE Produto ADD ValorCompra DECIMAL(10,2), ValorVenda DECIMAL(10,2)

SELECT * FROM Produto

UPDATE Produto SET ValorCompra = 1.5

UPDATE Produto SET ValorVenda = 2 WHERE ID = 1

UPDATE Produto SET ValorVenda = 0 WHERE ID = 4
--EXERCÍCIO
--EXECUTAR UPDATE SETANDO VALORVENDA = 1 PROS REGISTROS ONDE VALORVENDA = 0

--UPDATE Produto SET ValorVenda = 1 WHERE ValorVenda = 0
--github.com/williamwachholz/sql_02

SELECT  *, 
		ValorCompra + ValorVenda AS Soma,
		ValorCompra - ValorVenda AS Subtracao,
		ValorCompra * ValorVenda AS Multiplicacao,
	    ValorCompra / ValorVenda AS Divisao
FROM Produto

SELECT * FROM Produto WHERE Peso > 1 --maior
SELECT * FROM Produto WHERE Peso >= 1 --maior ou igual
SELECT * FROM Produto WHERE Peso < 1 --menor
SELECT * FROM Produto WHERE Peso <= 1 --menor ou igual
SELECT * FROM Produto WHERE Peso <> 0.1 --diferente
SELECT * FROM Produto WHERE Peso = 0.1 --igual

SELECT * FROM Produto WHERE (ValorCompra + ValorVenda) > 2 AND Peso > 3 --E
SELECT * FROM Produto WHERE (ValorCompra + ValorVenda) > 2 OR Peso > 3 --OU

SELECT * FROM Produto WHERE NOT Peso > 3 --onde peso não seja maior que 3
SELECT * FROM Produto WHERE Peso < 3 --onde peso menor que 3

SELECT * FROM Produto WHERE ID IN (1,2) --está em
SELECT * FROM Produto WHERE ID NOT IN (1,2) --não está em

SELECT * FROM Produto WHERE ValorVenda IS NULL --verificar nulo
SELECT * FROM Produto WHERE ValorVenda IS NOT NULL --não está nulo

SELECT SUBSTRING(Nome, 1, 3), Nome FROM Produto

SELECT LEN(Nome) AS 'Qtd. Caracteres', Nome FROM Produto

SELECT LOWER(Nome) AS TudoMinusculo, Nome FROM Produto

SELECT UPPER(Nome) AS TudoMaiusculo, Nome FROM Produto

--Exercício
--Retornar todos os produtos onde Nome inicia com 'L'
--e tamanho do Nome maior que 4
SELECT * FROM Produto 
WHERE SUBSTRING(Nome, 1, 1) = 'L' 
AND LEN(Nome) > 4
--Executar Update alterando o valor da coluna Nome
--para 'Lápis de Cor', do registro Nome = 'Lápis'
UPDATE Produto SET Nome = 'Lápis de Cor' WHERE Nome = 'Lápis'
--Retornar todos os produtos onde Nome inicia com 'L'
--ou tamanho do Nome maior ou igual a 6
SELECT * FROM Produto 
WHERE SUBSTRING(Nome, 1, 1) = 'L' 
OR LEN(Nome) >= 6

--para concatenar textos
SELECT CONCAT('Produto: ', Nome, ' | Peso: ', Peso) 
FROM Produto
--mesmo resultado que:
SELECT 'Produto: ' + Nome + ' | Peso: ' + CAST(Peso AS VARCHAR) 
FROM Produto
--mesmo resultado que:
SELECT CONCAT_WS(' ', 'Produto:', Nome, '| Peso:', Peso) 
FROM Produto

--retorna qtd registros da tabela
SELECT COUNT(*) FROM Produto
--retorna maior valor da coluna especificada
SELECT MAX(Peso) FROM Produto
--retorna menor valor da coluna especificada
SELECT MIN(Peso) FROM Produto

--exercício
--retornar todas as colunas do produto com o maior Nome
SELECT * FROM Produto 
WHERE LEN(Nome) = (SELECT MAX(LEN(Nome)) FROM Produto)

--retorna média da coluna
SELECT AVG(Peso) FROM Produto
--retorna soma da coluna
SELECT SUM(Peso) FROM Produto

UPDATE Produto SET ValorCompra = 2 WHERE ID = 1

--retorna valores distintos da coluna em ordenação descendente
SELECT DISTINCT(ValorCompra) FROM Produto
ORDER BY ValorCompra DESC

--Exercício
--retornar todos os registros da tabela produto
--ordenando pela coluna Peso de forma descendente
SELECT * FROM Produto ORDER BY Peso DESC
--retornar todos os registros da tabela produto
--onde peso maior que 0.1
--ordenando pela coluna Peso de forma ascendente
SELECT * FROM Produto 
WHERE Peso > 0.1
ORDER BY Peso ASC

--retornar o Nome e o Peso do primeiro registro da tabela
--ordenando pela coluna especificada
SELECT TOP(1) Nome, Peso FROM Produto 
ORDER BY Nome ASC

UPDATE Produto SET ValorVenda = 1 WHERE ID = 4
INSERT INTO Produto (ID, Nome, Peso, ValorCompra, ValorVenda)
VALUES (5, 'Caderno', 2, 1, 5)
INSERT INTO Produto (ID, Nome, Peso, ValorCompra, ValorVenda)
VALUES (6, 'Folhas', 2.6, 7, 7)

--exercício
--retornar todas as colunas da tabela Produto, 
--do primeiro registro
--onde ValorVenda maior ou igual que ValorCompra
--ordenando pelo Peso de forma descendente
SELECT TOP(1) * FROM Produto
WHERE ValorVenda >= ValorCompra
ORDER BY Peso DESC

--retorna data corrente
SELECT GETDATE()

--alterar estrutura tabela para incluir coluna DatAlteracao
ALTER TABLE Produto ADD DataAlteracao DATE
--preenchendo coluna criada com a data corrente
UPDATE Produto SET DataAlteracao = GETDATE()

--alterar estrutura da tabela para incluir DataAlteracao
--tipo DATETIME, para guardar a hora também
ALTER TABLE Produto ADD DataCriacao DATETIME
--preenchendo coluna criada com a data corrente
UPDATE Produto SET DataCriacao = GETDATE()

--setando a data de alteração 1 semana para frente de hoje
--passar a data no formato YYYY-MM-DD (Ano-Mês-Dia)
UPDATE Produto SET DataAlteracao = CAST('2019-10-07' AS DATE)
WHERE ID = 1

--exercício 1
--selecionar o registro da tabela produto que tenha
--a maior data de alteração
SELECT * FROM Produto
WHERE DataAlteracao = (SELECT MAX(DataAlteracao) FROM Produto)
--exercício 2
--selecionar os registros da tabela produto que tenham
--a data de criacao menor que a data corrente ---> GETDATE()
--e o valor de venda seja maior que o valor de compra
--e o peso seja maior que 3
SELECT * FROM Produto
WHERE DataCriacao < GETDATE()
AND ValorVenda > ValorCompra
AND Peso > 3
--exercício 3
--selecionar todos os registros da tabela Produto
--onde DataAlteracao = 2019-10-01 (USAR CAST)
--ordenando pelo nome de forma ascendente
SELECT * FROM Produto
WHERE DataAlteracao = CAST('2019-10-01' AS DATE)
ORDER BY Nome ASC

--para comparar somente a parte da DATA do tipo DATETIME
--pode se converter (CAST) para DATE 
SELECT * FROM Produto
WHERE CAST(DataCriacao AS DATE) = CAST(GETDATE() AS DATE)
ORDER BY Nome ASC

--último parametro da funcao CONVERT é o formato de apresentação
SELECT CONVERT(VARCHAR, DataCriacao, 103) FROM Produto
SELECT CONVERT(VARCHAR, DataCriacao, 104) FROM Produto
SELECT CONVERT(VARCHAR, DataCriacao, 121) FROM Produto

--se converter uma coluna DATE para DATETIME
--a parte que representa o tempo ficará zerada
SELECT CAST(DataAlteracao AS DATETIME) FROM Produto


CREATE TABLE Pedido (
	ID int primary key,
	Codigo varchar(10) not null,
	DataCriacao date
)

CREATE TABLE PedidoItem (
	ID int primary key,
	IDPedido int not null,
	IDProduto int not null,
	Quantidade decimal(10,2),
	constraint FK_Pedido foreign key(IDPedido) references Pedido(ID),
	constraint FK_Produto foreign key (IDProduto) references Produto (ID)
)



