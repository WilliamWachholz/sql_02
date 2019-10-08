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



INSERT INTO PedidoItem (ID, IDPedido, IDProduto, Quantidade)
VALUES (1, 1, 3, 10)

INSERT INTO PedidoItem (ID, IDPedido, IDProduto, Quantidade)
VALUES (2, 1, 5, 20)

--Exercício: inserir 3 registros na tabela PedidoItem
--para o Pedido ID = 2
--Produtos: ID = 1 (Lapis de Cor), Quantidade = 10
INSERT INTO PedidoItem (ID, IDPedido, IDProduto, Quantidade)
VALUES (3, 2, 1, 10)

--			ID = 3 (Livro), Quantidade = 20
INSERT INTO PedidoItem (ID, IDPedido, IDProduto, Quantidade)
VALUES (4, 2, 3, 20)

--			ID = 5 (Caneta), Quantidade = 30
INSERT INTO PedidoItem (ID, IDPedido, IDProduto, Quantidade)
VALUES (5, 2, 5, 30)

--JUNÇÃO DE TABELAS
SELECT  PedidoItem.ID,
		Produto.Nome,
		PedidoItem.Quantidade
FROM Pedido
INNER JOIN PedidoItem ON (PedidoItem.IDPedido = Pedido.ID)
INNER JOIN Produto ON (Produto.ID = PedidoItem.IDProduto)
WHERE Pedido.ID = 1

INSERT INTO Categoria (Nome) VALUES ('Fantasia')
INSERT INTO Categoria (Nome) VALUES ('Ficção')

INSERT INTO Livro (Titulo, DataPublicacao, 
Edicao, CodigoSequencial, IDCategoria) 
VALUES ('O Hobbit', CAST('2010-01-01' AS DATE), 12, 4232, 1)   

SELECT  Livro.Titulo,
		Categoria.Nome
FROM Livro
INNER JOIN Categoria ON (Categoria.ID = Livro.IDCategoria)

SELECT  Livro.Titulo,
		Categoria.Nome
FROM Categoria
INNER JOIN Livro ON (Categoria.ID = Livro.IDCategoria)


SELECT  Livro.Titulo,
		Categoria.Nome
FROM Livro
LEFT JOIN Categoria ON (Categoria.ID = Livro.IDCategoria)

SELECT  Livro.Titulo,
		Categoria.Nome
FROM Categoria
LEFT JOIN Livro ON (Categoria.ID = Livro.IDCategoria)

INSERT INTO Livro (Titulo, DataPublicacao, 
Edicao, CodigoSequencial, IDCategoria) 
VALUES ('O Senhor dos Anéis', CAST('2010-01-01' AS DATE), 20, 5234423, 1)  

SELECT  Livro.Titulo,
		Categoria.Nome
FROM Categoria
LEFT JOIN Livro ON (Categoria.ID = Livro.IDCategoria)



SELECT  Livro.Titulo,
		Categoria.Nome
FROM Categoria
RIGHT JOIN Livro ON (Categoria.ID = Livro.IDCategoria)


SELECT  Livro.Titulo,
		Categoria.Nome
FROM Livro
RIGHT JOIN Categoria ON (Categoria.ID = Livro.IDCategoria)


SELECT  Livro.Titulo,
		Categoria.Nome
FROM Livro
INNER JOIN Categoria ON (Categoria.ID = Livro.IDCategoria)
UNION
SELECT  'Nenhum Livro',
		'Nenhuma Categoria'

INSERT INTO Autor VALUES ('Edgar Allan', 'Poe', CAST('1873-09-17' AS DATE))


INSERT INTO Livro (Titulo, DataPublicacao, 
Edicao, CodigoSequencial, IDCategoria) 
VALUES ('Contos', CAST('2010-01-01' AS DATE), 10, 2123, 1)  

INSERT INTO LivroAutor VALUES (3, 1)

SELECT  Livro.Titulo AS Titulo,
		Categoria.Nome AS Categoria,
		'' AS Autor
FROM Livro
INNER JOIN Categoria ON (Categoria.ID = Livro.IDCategoria)
UNION
SELECT  Livro.Titulo AS Titulo,
		'' AS Categoria,
		Autor.Nome AS Autor
FROM Livro
INNER JOIN LivroAutor ON (LivroAutor.IDLivro = Livro.ID)
INNER JOIN Autor ON (Autor.ID = LivroAutor.IDAUtor)

--exercicio

--criar tabela estoque com campos id pk identity, 
--quantidadeEstoque decimal(10,2), 
--dataPosicao Date, 
--IDProduto int

--criar chave estrangeira referenciado a tabela Produto

CREATE TABLE Estoque (
	ID INT Primary Key Identity,
	QuantidadeEstoque DECIMAL(10,2),
	DataPosicao DATE,
	IDProduto INT,
	CONSTRAINT FK_Produto2 FOREIGN KEY(IDProduto) REFERENCES Produto(ID)
)

--ALTER TABLE Estoque ADD CONSTRAINT FK_Produto2 FOREIGN KEY(IDProduto) REFERENCES Produto(ID)

--inserir registros para caneta, caderno e livro na tabela Estoque
INSERT INTO Estoque (QuantidadeEstoque, DataPosicao, IDProduto) VALUES (10, GETDATE(), 3)
INSERT INTO Estoque VALUES (30, GETDATE(), 4)
INSERT INTO Estoque VALUES (50, GETDATE(), 5)

select * from Estoque

--fazer select para trazer nome do produto, quantidadeEstoque e dataPosicao. 
--Trazer somente os produtos que tem estoque

SELECT  Produto.Nome,
		Estoque.QuantidadeEstoque,		
		Estoque.DataPosicao
FROM Produto
INNER JOIN Estoque ON (Estoque.IDProduto = Produto.ID)

--fazer o mesmo select acima, mas trazendo todos os produtos, 
--independente de ter estoque. 

SELECT  Produto.Nome,
		ISNULL(Estoque.QuantidadeEstoque, 0) AS 'Quantidade Estoque',		
		ISNULL(Estoque.DataPosicao, GETDATE()) AS 'Data Posição' 
FROM Produto
LEFT JOIN Estoque ON (Estoque.IDProduto = Produto.ID)

--caso não seja necessário mostrar colunas da tabela Estoque,
--apenas verificar se o produto tem estoque correspondente,
--pode ser usado o comando EXISTS
SELECT *
FROM Produto
WHERE EXISTS (SELECT * FROM Estoque WHERE Estoque.IDProduto = Produto.ID)

--para filtrar período de data, usa-se o comando BETWEEN
SELECT  Produto.Nome,
		Estoque.QuantidadeEstoque,		
		Estoque.DataPosicao
FROM Produto
INNER JOIN Estoque ON (Estoque.IDProduto = Produto.ID)
WHERE Estoque.DataPosicao 
BETWEEN CAST('2019-09-01' AS DATE) AND CAST('2019-10-30' AS DATE)


select * from estoque

select sum(QuantidadeEstoque)
from estoque
where IDProduto = 3


select  Produto.Nome,
		sum(Estoque.QuantidadeEstoque)
from Estoque
inner join Produto on (Produto.ID = Estoque.IDProduto)
group by Produto.Nome

select  Produto.Nome,
		Estoque.QuantidadeEstoque
from Estoque
inner join Produto on (Produto.ID = Estoque.IDProduto)

select  sum(Estoque.QuantidadeEstoque)
from Estoque
inner join Produto on (Produto.ID = Estoque.IDProduto)
group by Produto.Nome


--inserir registro na LivroTag relacionando 
--registro da tabela tag com o correspondente na tabela Livro
INSERT INTO Tag VALUES ('HOB') --O Hobbit
INSERT INTO Tag VALUES ('SHA') --O Senhor dos Anéis

INSERT INTO LivroTag VALUES (1, 1)
INSERT INTO LivroTag VALUES (2, 2)

--retornar titulo do livro e nome da tag
--somente dos livros que tenham tag correspondente
SELECT  Livro.Titulo,
		Tag.Nome
FROM Livro
INNER JOIN LivroTag ON (LivroTag.IDLivro = Livro.ID)
INNER JOIN Tag ON (Tag.ID = LivroTag.IDTag)

INSERT INTO Tag VALUES ('HOB2') --O Hobbit
INSERT INTO LivroTag VALUES (1, 3)

--GROUP BY
SELECT  Livro.Titulo,
		COUNT(Tag.Nome)
FROM Livro
INNER JOIN LivroTag ON (LivroTag.IDLivro = Livro.ID)
INNER JOIN Tag ON (Tag.ID = LivroTag.IDTag)
GROUP BY Livro.Titulo


--HAVING
SELECT  Livro.Titulo,
		COUNT(Tag.Nome)
FROM Livro
INNER JOIN LivroTag ON (LivroTag.IDLivro = Livro.ID)
INNER JOIN Tag ON (Tag.ID = LivroTag.IDTag)
GROUP BY Livro.Titulo
HAVING COUNT(Tag.Nome) > 1

--VIEWS
CREATE VIEW LivroComCategoria
AS
SELECT  Livro.Titulo,
		Categoria.Nome Categoria
from Livro
INNER JOIN Categoria ON (Livro.IDCategoria = Categoria.ID)

select * 
from LivroComCategoria

--criar select que partindo da view LivroComCategoria
--traga LivroComCategoria.Titulo, LivroComCategoria.Categoria
--e Autor.Nome
--somente dos livros que tem autor relacionado

SELECT  LivroComCategoria.Titulo,
		LivroComCategoria.Categoria,
		Autor.Nome
FROM LivroComCategoria
INNER JOIN Livro ON (Livro.Titulo = LivroComCategoria.Titulo)
INNER JOIN LivroAutor ON (LivroAutor.IDLivro = Livro.ID)
INNER JOIN Autor ON (Autor.ID = LivroAutor.IDAutor)

ALTER VIEW LivroComCategoria
AS
SELECT  Livro.ID AS IDLivro,
		Livro.Titulo,
		Categoria.Nome Categoria
from Livro
INNER JOIN Categoria ON (Livro.IDCategoria = Categoria.ID)

SELECT  LivroComCategoria.Titulo,
		LivroComCategoria.Categoria,
		Autor.Nome
FROM LivroComCategoria
INNER JOIN Livro ON (Livro.ID = LivroComCategoria.IDLivro)
INNER JOIN LivroAutor ON (LivroAutor.IDLivro = Livro.ID)
INNER JOIN Autor ON (Autor.ID = LivroAutor.IDAutor)

--DROP VIEW LivroComCategoria

--INDICE
CREATE INDEX IDX_Socio ON Locacao(IDSocio)

DROP INDEX idx_socio ON Locacao

--STORED PROCEDURE
ALTER PROC InserirTag @Nome VARCHAR(200), @Par2 INT
AS
BEGIN
	INSERT INTO Tag (Nome) VALUES (@Nome)	

	DECLARE @UltimaTag INT;

	SELECT @UltimaTag = MAX(ID) FROM Tag;

	INSERT INTO LivroTag 
	SELECT  Livro.ID,
			@UltimaTag
	FROM Livro
END

EXECUTE InserirTag 'EXE'

select * from Tag
select * from LivroTag

--excluir SP (Stored Procedure)
--DROP PROC InserirTag


--CREATE PROC MinhaProc @Par1 VARCHAR(200), @Par2 INT

--criar uma SP para inserir novos livros
--tendo como parâmetros titulo, datapublicacao, edicao,
--codigoSequencial, idcategoria
CREATE PROC InserirLivro @Titulo VARCHAR(100), 
@DataPublicacao DATE, @Edicao VARCHAR(100),
@CodigoSequencial INT, @IDCategoria INT
AS
BEGIN
	INSERT INTO Livro VALUES
	(@Titulo, @DataPublicacao, @Edicao, @CodigoSequencial, @IDCategoria)
END

EXECUTE InserirLivro 'A Menina Que Roubava Livros', 
'2017-10-10', 3, 4903, 1

EXECUTE InserirLivro 'O milagre da manhã', 
'2016-05-03', 1, 277, 1

EXECUTE InserirLivro 'O poder do hábito', 
'2015-02-20', 2, 546, 1

select * from Socio

CREATE PROC InserirSocio @Nome VARCHAR(100), 
@Sobrenome VARCHAR(100), @DataNascimento DATE,
@Profissao VARCHAR(100), @Sexo VARCHAR(1)
AS
BEGIN
	INSERT INTO Socio VALUES
	(@Nome, @Sobrenome, @DataNascimento, @Profissao, @Sexo)
END

EXEC InserirSocio 'João', 'Silva', '1980-07-28', 'Analista', 'M'
EXEC InserirSocio 'Maria', 'Silva', '1992-02-13', 'Advogada', 'F'

CREATE PROC InserirAutor @Nome VARCHAR(100), @Sobrenome VARCHAR(100), 
@DataNascimento DATE
AS
BEGIN
	INSERT INTO Autor VALUES (@Nome, @Sobrenome, @DataNascimento)
END

EXEC InserirAutor 'Markus', 'Zusak', ''
EXEC InserirAutor 'Hal', 'Elrod', ''
EXEC InserirAutor 'Charles', 'Duhigg', ''

SELECT * FROM lIVRO where id> 3
SELECT * FROM AUTOR where id>2

INSERT INTO LivroAutor VALUES (4, 4)
INSERT INTO LivroAutor VALUES (5, 5)
INSERT INTO LivroAutor VALUES (6, 6)

INSERT INTO Locacao VALUES ('2019-10-01', '2019-10-11', NULL, 1)
INSERT INTO Locacao VALUES ('2019-10-05', '2019-10-15', NULL, 2)

INSERT INTO LocacaoLivro VALUES (4, 1)
INSERT INTO LocacaoLivro VALUES (5, 1)
INSERT INTO LocacaoLivro VALUES (6, 2)

--criar select partindo da tabela Locacao
--retornar dataretirada, dataprevisaodevolucao, datadevolucao,
--nome socio, nome autor, titulo do livro e data publicacao do livro

SELECT  Locacao.DataRetirada AS DataRetirada,
		Locacao.DataPrevisaoDevolucao  AS DataPrevisao,
		Locacao.DataDevolucao AS DataDevolucao,
		Socio.Nome + ' ' + Socio.Sobrenome AS Socio,
		Autor.Nome + ' ' + Autor.Sobrenome AS Autor,
		Livro.Titulo AS Titulo,
		Livro.DataPublicacao AS DataPublicacao
FROM Locacao
INNER JOIN Socio ON (Socio.ID = Locacao.IDSocio) 
INNER JOIN LocacaoLivro ON (LocacaoLivro.IDLocacao = Locacao.ID)
INNER JOIN Livro ON (Livro.ID = LocacaoLivro.IDLivro)
INNER JOIN LivroAutor ON (LivroAutor.IDLivro = Livro.ID)
INNER JOIN Autor ON (Autor.ID = LivroAutor.IDAutor)










