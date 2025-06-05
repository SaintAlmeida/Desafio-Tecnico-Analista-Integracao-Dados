
# ✅ Parte 3 – Estratégia de Validação com o Cliente

Esta etapa descreve uma abordagem prática e orientada para validar os dados do mês de **Fevereiro de 2025** com o cliente, garantindo clareza, confiança e precisão na análise final.

## ✍️ Parte 3 – Estratégia de Validação com o Cliente

### 🔍 1. Principais pontos a validar com o cliente

- Confirmação do **volume total de vendas e pedidos** no mês.
- Verificação de **produtos com pendência de entrega** ou que **não foram recebidos**.
- Análise de **consumo versus pedidos realizados** para identificar divergências.
- Validação das **descrições e códigos dos produtos** utilizados na operação.
- Identificação de **possíveis duplicidades ou lacunas** nas datas e quantidades.
- Confirmação de que as **quantidades entregues e pendentes** estão corretas.

---

### 🛠️ 2. Técnicas para garantir exatidão e precisão dos dados

- **Comparação cruzada** entre as tabelas `venda`, `pedido_compra` e `entradas_mercadoria`.
- Uso de **filtros por período (mês)** para isolar os dados de fevereiro de 2025.
- **Validação de totalizadores** (SOMAS, contagens, médias) comparando com relatórios paralelos do cliente.
- Aplicação de **`DISTINCT` e `GROUP BY`** para detectar duplicidades ou inconsistências.
- **Tratamento de valores nulos** ou faltantes antes de calcular métricas.
- Verificação do **relacionamento entre IDs** (ex: `produto_id`, `ordem_compra`) para garantir integridade referencial.

---

### 📊 3. Consultas SQL de apoio para validação

#### 🔸 Total de consumo por produto (fevereiro/2025)

```sql
SELECT 
    produto_id,
    SUM(qtde_vendida) AS total_consumo
FROM 
    public.venda
WHERE 
    DATE_TRUNC('month', data_emissao) = DATE '2025-02-01'
GROUP BY 
    produto_id;
```

#### 🔸 Total de pedidos realizados (fevereiro/2025)

```sql
SELECT 
    produto_id,
    descricao_produto,
    SUM(qtde_pedida) AS total_pedida
FROM 
    public.pedido_compra
WHERE 
    DATE_TRUNC('month', data_pedido) = DATE '2025-02-01'
GROUP BY 
    produto_id, descricao_produto;
```

#### 🔸 Produtos com pendência de entrega

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
    qtde_pendente > 0
  AND DATE_TRUNC('month', data_pedido) = DATE '2025-02-01';
```

#### 🔸 Produtos requisitados mas não consumidos e não recebidos

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

---

### ✅ Considerações finais

Durante a reunião com o cliente, o objetivo principal é **transmitir confiança na qualidade dos dados**. Para isso, além das consultas acima, é essencial:

- Apresentar os **números totais e percentuais** com clareza.
- Estar preparado para **comparações rápidas** e ajustes se necessário.
- Documentar todas as validações feitas com prints ou exportações, se solicitado.

