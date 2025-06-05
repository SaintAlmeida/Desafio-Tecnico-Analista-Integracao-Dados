# PostgreSQL – Criação e Popularização de Tabelas para Análise de Dados

Este repositório contém os scripts SQL para criação e inserção de dados em três tabelas: `venda`, `pedido_compra` e `entradas_mercadoria`, com o objetivo de simular um cenário de análise de consumo e movimentação de mercadorias em uma empresa.

## 📦 Estrutura das Tabelas

### 1. Tabela `venda`

```sql
CREATE TABLE public.venda(
    venda_id int8 NOT NULL,
    data_emissao date NOT NULL,
    horariomov varchar(8) DEFAULT '00:00:00' NOT NULL,
    produto_id varchar(25) DEFAULT '' NOT NULL,
    qtde_vendida float8,
    valor_unitario numeric(12, 4) DEFAULT 0 NOT NULL,
    filial_id int8 DEFAULT 1 NOT NULL,
    item int4 DEFAULT 0 NOT NULL,
    unidade_medida varchar(3),
    CONSTRAINT pk_consumo PRIMARY KEY (filial_id, venda_id, data_emissao, produto_id, item, horariomov)
);
```

### 2. Tabela `pedido_compra`

```sql
CREATE TABLE public.pedido_compra(
    pedido_id float8 DEFAULT 0 NOT NULL,
    data_pedido date,
    item float8 DEFAULT 0 NOT NULL,
    produto_id varchar(25) DEFAULT '0' NOT NULL,
    descricao_produto varchar(255),
    ordem_compra float8 DEFAULT 0 NOT NULL,
    qtde_pedida float8,
    filial_id int4,
    data_entrega date,
    qtde_entregue float8 DEFAULT 0 NOT NULL,
    qtde_pendente float8 DEFAULT 0 NOT NULL,
    preco_compra float8,
    fornecedor_id int4 DEFAULT 0,
    CONSTRAINT pedido_compra_pkey PRIMARY KEY (pedido_id, produto_id, item)
);
```

### 3. Tabela `entradas_mercadoria`

> ⚠️ **Observação:** o script original fornecido para esta tabela estava com erro, pois a coluna `ordem_compra` era usada como chave primária, mas **não estava declarada**. Por isso, a definição da tabela foi ajustada com a adição da coluna `ordem_compra` com o tipo `float8` para manter compatibilidade com a tabela `pedido_compra`.

```sql
CREATE TABLE public.entradas_mercadoria (
    data_entrada date,
    nro_nfe varchar(255) NOT NULL,
    item float8 DEFAULT 0 NOT NULL,
    produto_id varchar(25) DEFAULT '0' NOT NULL,
    descricao_produto varchar(255),
    qtde_recebida float8,
    filial_id int4,
    custo_unitario numeric(12, 4) DEFAULT 0 NOT NULL,
    ordem_compra float8 DEFAULT 0 NOT NULL,
    CONSTRAINT entradas_mercadoria_pkey PRIMARY KEY (ordem_compra, item, produto_id, nro_nfe)
);
```

## 📝 Inserção de Dados

Foram inseridas **20 linhas de dados fictícios** em cada tabela, respeitando os relacionamentos, formatos e chaves primárias. Os dados representam movimentações reais. Seguem os Inserts: 

```sql
INSERT INTO public.venda (
    venda_id, data_emissao, horariomov, produto_id, qtde_vendida,
    valor_unitario, filial_id, item, unidade_medida
) VALUES
(1001, '2025-01-05', '08:00:00', 'P001', 10, 5.00, 1, 1, 'UN'),
(1002, '2025-01-08', '08:30:00', 'P002', 8, 7.50, 1, 1, 'UN'),
(1003, '2025-01-12', '09:15:00', 'P003', 12, 4.00, 1, 1, 'CX'),
(1004, '2025-01-20', '10:00:00', 'P004', 15, 6.50, 1, 1, 'UN'),
(1005, '2025-01-25', '10:45:00', 'P005', 6, 3.20, 1, 1, 'UN'),
(1006, '2025-01-30', '11:00:00', 'P006', 9, 8.00, 1, 1, 'UN'),
(1007, '2025-02-03', '08:20:00', 'P007', 5, 9.10, 1, 1, 'UN'),
(1008, '2025-02-07', '09:00:00', 'P008', 7, 10.00, 1, 1, 'UN'),
(1009, '2025-02-10', '09:30:00', 'P009', 4, 2.50, 1, 1, 'UN'),
(1010, '2025-02-12', '10:00:00', 'P010', 3, 1.99, 1, 1, 'UN'),
(1011, '2025-02-15', '10:30:00', 'P011', 11, 5.25, 1, 1, 'UN'),
(1012, '2025-02-18', '11:00:00', 'P012', 2, 7.10, 1, 1, 'UN'),
(1013, '2025-02-20', '11:30:00', 'P013', 9, 8.80, 1, 1, 'UN'),
(1014, '2025-02-25', '12:00:00', 'P014', 5, 6.70, 1, 1, 'UN'),
(1015, '2025-03-01', '08:15:00', 'P015', 13, 9.00, 1, 1, 'UN'),
(1016, '2025-03-05', '08:45:00', 'P016', 4, 3.90, 1, 1, 'UN'),
(1017, '2025-03-10', '09:00:00', 'P017', 6, 2.10, 1, 1, 'UN'),
(1018, '2025-03-15', '09:30:00', 'P018', 8, 4.40, 1, 1, 'UN'),
(1019, '2025-03-20', '10:00:00', 'P019', 10, 5.50, 1, 1, 'UN'),
(1020, '2025-03-28', '10:30:00', 'P020', 14, 6.00, 1, 1, 'UN');

```

```sql
INSERT INTO public.pedido_compra (
    pedido_id, data_pedido, item, produto_id, descricao_produto,
    ordem_compra, qtde_pedida, filial_id, data_entrega,
    qtde_entregue, qtde_pendente, preco_compra, fornecedor_id
) VALUES
(2001, '2025-01-04', 1, 'P001', 'Sabão', 3001, 20, 1, '2025-01-06', 10, 10, 4.90, 101),
(2002, '2025-01-09', 1, 'P002', 'Detergente', 3002, 25, 1, '2025-01-11', 25, 0, 5.40, 102),
(2003, '2025-01-14', 1, 'P003', 'Esponja', 3003, 30, 1, '2025-01-16', 10, 20, 2.00, 101),
(2004, '2025-01-19', 1, 'P004', 'Álcool', 3004, 18, 1, '2025-01-21', 18, 0, 3.20, 103),
(2005, '2025-01-24', 1, 'P005', 'Desinfetante', 3005, 20, 1, '2025-01-26', 10, 10, 6.00, 101),
(2006, '2025-01-29', 1, 'P006', 'Luva', 3006, 22, 1, '2025-01-31', 20, 2, 1.50, 104),
(2007, '2025-02-02', 1, 'P007', 'Máscara', 3007, 40, 1, '2025-02-04', 40, 0, 0.90, 105),
(2008, '2025-02-06', 1, 'P008', 'Sabonete', 3008, 35, 1, '2025-02-08', 30, 5, 2.10, 106),
(2009, '2025-02-10', 1, 'P009', 'Papel Toalha', 3009, 50, 1, '2025-02-12', 40, 10, 4.50, 102),
(2010, '2025-02-13', 1, 'P010', 'Pano Limpeza', 3010, 15, 1, '2025-02-15', 15, 0, 3.90, 101),
(2011, '2025-02-16', 1, 'P011', 'Água Sanitária', 3011, 20, 1, '2025-02-18', 10, 10, 3.30, 104),
(2012, '2025-02-19', 1, 'P012', 'Desodorante', 3012, 18, 1, '2025-02-21', 18, 0, 7.10, 105),
(2013, '2025-02-22', 1, 'P013', 'Sabonete Líquido', 3013, 25, 1, '2025-02-24', 25, 0, 5.70, 106),
(2014, '2025-02-26', 1, 'P014', 'Cloro', 3014, 16, 1, '2025-02-28', 15, 1, 4.10, 101),
(2015, '2025-03-01', 1, 'P015', 'Detergente Neutro', 3015, 30, 1, '2025-03-03', 20, 10, 6.20, 102),
(2016, '2025-03-05', 1, 'P016', 'Saco Lixo', 3016, 28, 1, '2025-03-07', 28, 0, 2.30, 103),
(2017, '2025-03-09', 1, 'P017', 'Rodo', 3017, 12, 1, '2025-03-11', 12, 0, 8.80, 104),
(2018, '2025-03-13', 1, 'P018', 'Vassoura', 3018, 14, 1, '2025-03-15', 14, 0, 9.10, 105),
(2019, '2025-03-17', 1, 'P019', 'Balde', 3019, 10, 1, '2025-03-19', 10, 0, 11.00, 106),
(2020, '2025-03-25', 1, 'P020', 'Multiuso', 3020, 19, 1, '2025-03-27', 9, 10, 4.70, 101);

```

```sql
INSERT INTO public.entradas_mercadoria (
    data_entrada, nro_nfe, item, produto_id, descricao_produto,
    qtde_recebida, filial_id, custo_unitario, ordem_compra
) VALUES
('2025-01-03', 'NF001', 1, 'P001', 'Sabão', 10, 1, 4.90, 3001),
('2025-01-07', 'NF002', 1, 'P002', 'Detergente', 25, 1, 5.40, 3002),
('2025-01-11', 'NF003', 1, 'P003', 'Esponja', 10, 1, 2.00, 3003),
('2025-01-15', 'NF004', 1, 'P004', 'Álcool', 18, 1, 3.20, 3004),
('2025-01-19', 'NF005', 1, 'P005', 'Desinfetante', 10, 1, 6.00, 3005),
('2025-01-23', 'NF006', 1, 'P006', 'Luva', 20, 1, 1.50, 3006),
('2025-02-03', 'NF007', 1, 'P007', 'Máscara', 40, 1, 0.90, 3007),
('2025-02-07', 'NF008', 1, 'P008', 'Sabonete', 30, 1, 2.10, 3008),
('2025-02-10', 'NF009', 1, 'P009', 'Papel Toalha', 40, 1, 4.50, 3009),
('2025-02-13', 'NF010', 1, 'P010', 'Pano Limpeza', 15, 1, 3.90, 3010),
('2025-02-16', 'NF011', 1, 'P011', 'Água Sanitária', 10, 1, 3.30, 3011),
('2025-02-19', 'NF012', 1, 'P012', 'Desodorante', 18, 1, 7.10, 3012),
('2025-02-22', 'NF013', 1, 'P013', 'Sabonete Líquido', 25, 1, 5.70, 3013),
('2025-02-25', 'NF014', 1, 'P014', 'Cloro', 15, 1, 4.10, 3014),
('2025-02-28', 'NF015', 1, 'P015', 'Detergente Neutro', 20, 1, 6.20, 3015),
('2025-03-03', 'NF016', 1, 'P016', 'Saco Lixo', 28, 1, 2.30, 3016),
('2025-03-07', 'NF017', 1, 'P017', 'Rodo', 12, 1, 8.80, 3017),
('2025-03-15', 'NF018', 1, 'P018', 'Vassoura', 14, 1, 9.10, 3018),
('2025-03-22', 'NF019', 1, 'P019', 'Balde', 10, 1, 11.00, 3019),
('2025-03-29', 'NF020', 1, 'P020', 'Multiuso', 9, 1, 4.70, 3020);

```
## 📁 Estrutura do Projeto

- `create_tables.sql` – Script de criação das três tabelas.
- `insert_data.sql` – Script de inserção dos dados fictícios (20 registros por tabela).


## 🚀 Requisitos

- PostgreSQL instalado localmente
- Ferramenta de gerenciamento : **DBeaver** 
- Executar os scripts em ordem: primeiro `create_tables.sql`, depois `insert_data.sql`
