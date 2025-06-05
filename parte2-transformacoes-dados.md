
# 🔄 Parte 2 – Transformações de Dados

Este arquivo contém a consulta SQL correspondente à **Parte 2** do desafio técnico, que solicita transformações nos dados de **pedidos de compra** e **vendas** com os seguintes requisitos:

## ✍️ Parte 2 – Transformações de Dados

1. Concatenar os campos `produto_id` e `descricao_produto` (onde houver) no formato `produto_id - descricao_produto`;
2. Transformar o campo de datas para o formato `DD/MM/YYYY`;
3. Retornar os dados filtrando apenas os produtos **requisitados mais de 10 vezes** no período.

---

### ✅ Consulta SQL

```sql
-- Pedidos de Compra
SELECT 
    CONCAT(pc.produto_id, ' - ', pc.descricao_produto) AS produto,
    SUM(pc.qtde_pedida) AS qtde_requisitada,
    TO_CHAR(pc.data_pedido, 'DD/MM/YYYY') AS data_solicitacao
FROM 
    public.pedido_compra pc
GROUP BY 
    pc.produto_id, pc.descricao_produto, pc.data_pedido
HAVING 
    SUM(pc.qtde_pedida) > 10

UNION ALL

-- Vendas com descrição vinda da tabela entradas_mercadoria
SELECT 
    CONCAT(v.produto_id, ' - ', em.descricao_produto) AS produto,
    SUM(v.qtde_vendida) AS qtde_requisitada,
    TO_CHAR(v.data_emissao, 'DD/MM/YYYY') AS data_solicitacao
FROM 
    public.venda v
JOIN 
    public.entradas_mercadoria em 
    ON v.produto_id = em.produto_id
GROUP BY 
    v.produto_id, em.descricao_produto, v.data_emissao
HAVING 
    SUM(v.qtde_vendida) > 10;
```
