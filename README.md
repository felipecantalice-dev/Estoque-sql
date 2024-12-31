# Estoque
Lógica para realizar o cadastro e armazenamento de grandes volumes do mesmo produto, dentro uma empresa dividida em setores, com áreas amplas.


## Banco de dados
```sql
CREATE TABLE produto(
    id INT NOT NULL,
    nome VARCHAR(120),
    PRIMARY KEY(id)
);


CREATE TABLE setor(
    id INT NOT NULL,
    simbolo CHAR(2),
    PRIMARY KEY(id)
);

CREATE TABLE estoque(
    id INT NOT NULL,
    idSetor INT,
    preco float,
  	quantidade INT NOT NULL,
    PRIMARY KEY(id),
    FOREIGN KEY (idSetor) REFERENCES setor(id) ON DELETE CASCADE
);

CREATE TABLE estoque_produto(
    id INT NOT NULL,
    idEstoque INT NOT NULL,
    idProduto INT NOT NULL,
    PRIMARY KEY(id),
    FOREIGN KEY (idEstoque) REFERENCES estoque(id) ON DELETE CASCADE,
    FOREIGN KEY (idProduto) REFERENCES produto(id) ON DELETE CASCADE
);

```

### Dados amostra
```sql

-- Dados para a tabela produto
INSERT INTO produto (id, nome) 
	VALUES 
    (1, 'Camiseta'), 
    (2, 'Calça Jeans'), 
    (3, 'Tênis'), 
    (4, 'Jaqueta'), 
    (5, 'Boné'),
    (6, 'Óculos de Sol'), 
    (7, 'Relógio'), 
    (8, 'Bolsa'), 
    (9, 'Sapato'), 
    (10, 'Meia'), 
    (11, 'Cinto'), 
    (12, 'Saia'), 
    (13, 'Blusa'), 
    (14, 'Chapéu'), 
    (15, 'Luvas');

-- Dados para a tabela setor
INSERT INTO setor (id, simbolo) VALUES (1, 'A');
INSERT INTO setor (id, simbolo) VALUES (2, 'B');
INSERT INTO setor (id, simbolo) VALUES (3, 'C');

-- Dados para a tabela estoque
INSERT INTO estoque (id, idSetor, preco, quantidade) VALUES 
	(1, 1, 12.00, 10), (2, 2, 20.00, 23), (3, 3, 32.00, 17), 
    (4, 4, 17.00, 40), (5, 5, 50.00, 26), (6, 1, 49.99, 12), 
    (7, 2, 89.90, 37), (8, 3, 120.00, 7), (9, 3, 199.99, 39), 
    (10, 2, 25.00, 38);

-- Dados para a tabela estoque_produto
INSERT INTO estoque_produto (id, idEstoque, idProduto) VALUES 
	(1, 1, 1), (2, 2, 2), (3, 3, 3),
    (4, 4, 4), (5, 5, 5), (6, 1, 6), 
    (7, 2, 7), (8, 3, 5), (9, 4, 9), 
    (10, 5, 10),(11, 1, 11), (12, 2, 12), 
    (13, 3, 13), (14, 4, 14), (15, 5, 15);

```


```sql
-- Quantidade de estoques de um produto
SELECT ep.id, ep.idEstoque, s.simbolo, p.nome
	FROM estoque_produto ep
    INNER JOIN estoque e on e.id = ep.idEstoque
    INNER JOIN produto p on p.id = ep.idProduto
    INNER JOIN setor s on s.id = e.idSetor
    WHERE ep.idProduto = 2;
```