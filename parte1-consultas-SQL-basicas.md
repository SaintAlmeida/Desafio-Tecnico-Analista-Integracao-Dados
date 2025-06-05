
# 📊 Parte 1 – Consultas SQL Básicas

Este arquivo contém as três consultas SQL relacionadas à primeira etapa do desafio técnico, conforme o enunciado:

## ✍️ Parte 1 – Consultas SQL Básicas

### 1.1 – Consumo por produto e mês

Consulta que traz o **total de consumo** de cada **produto** no mês de **fevereiro de 2025**.

```sql
SELECT 
    produto_id,
    SUM(qtde_vendida) AS total_consumo,
    EXTRACT(MONTH FROM data_emissao) AS "mês"
FROM 
    public.venda
WHERE 
    DATE_TRUNC('month', data_emissao) = DATE '2025-02-01'
GROUP BY 
    produto_id,
    data_emissao
ORDER BY 
    total_consumo DESC;
```

---

### 1.2 – Produtos com requisição pendente

Consulta que lista os produtos que foram **requisitados, mas não recebidos**.

```sql
SELECT 
    produto_id,
    descricao_produto,
    qtde_pedida,
    qtde_entregue,
    qtde_pendente
FROM 
    public.pedido_compra
WHERE 
    qtde_pendente > 0;
```

---

### 1.3 – Produtos não consumidos e não recebidos

Consulta que lista **produtos que foram requisitados, mas não consumidos e não recebidos**, no mês de **fevereiro de 2025**.

```sql
SELECT DISTINCT pc.produto_id, pc.descricao_produto
FROM 
    public.pedido_compra pc
WHERE 
    DATE_TRUNC('month', pc.data_pedido) = DATE '2025-02-01'
    AND pc.produto_id NOT IN (
        SELECT DISTINCT produto_id 
        FROM public.venda 
        WHERE DATE_TRUNC('month', data_emissao) = DATE '2025-02-01'
    )
    AND pc.produto_id NOT IN (
        SELECT DISTINCT produto_id 
        FROM public.entradas_mercadoria 
        WHERE DATE_TRUNC('month', data_entrada) = DATE '2025-02-01'
    );
```
