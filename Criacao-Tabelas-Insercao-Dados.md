
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

Foram inseridas **20 linhas de dados fictícios** em cada tabela, respeitando os relacionamentos, formatos e chaves primárias. Os dados representam movimentações reais.


## 📁 Estrutura do Projeto

- `create_tables.sql` – Script de criação das três tabelas.
- `insert_data.sql` – Script de inserção dos dados fictícios (20 registros por tabela).


## 🚀 Requisitos

- PostgreSQL instalado localmente
- Ferramenta de gerenciamento : **DBeaver** 
- Executar os scripts em ordem: primeiro `create_tables.sql`, depois `insert_data.sql`
