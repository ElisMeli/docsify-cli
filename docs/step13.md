### Step 13 - Base Coeficiente Complementar

🔹 Step 13 – `LK_BASE_COEFICIENTE_COMPLEMENTAR_CUSTOS`

✅ **Objetivo:**

Construir uma base com coeficientes comparativos entre custos regulares e complementares, além de cálculos específicos para veículos alugados, consolidando indicadores financeiros por período para ajustes de modelagem orçamentária.

---

⚙️ **Fonte de dados:**

**Tabelas utilizadas:**
- `meli-bi-data.WHOWNER.BT_SHP_LG_PRE_INVOICE_DETAIL`
- `meli-bi-data.WHOWNER.BT_SHP_LG_PRE_INVOICE_ROUTES`
- `meli-bi-data.WHOWNER.BT_SHP_LG_PRE_INVOICE_HEADER`

**Descrição:** Essas tabelas armazenam detalhes de pré-faturamento, rotas e cabeçalhos de cobrança logística para análises de custos por tipo de operação.

**Filtros aplicados:**
- `SIT_SITE_ID = 'MLB'`
- `SHP_LG_PRE_INVOICE_HEADER_STEP_TYPE = 'last_mile'`
- `SHP_LG_PRE_INVOICE_HEADER_STATUS_NAME NOT IN ('canceled')`
- `SHP_LG_PRODUCT_TYPE NOT IN ('crowd')`
- `SHP_LG_PRE_INVOICE_HEADER_TOTAL_COST > 0`

---

📐 **Transformações e Seleções:**

| **Coluna no Input**               | **Coluna no Output**               | **Descrição**                                                         |
| :--------------------------------:| :---------------------------------:| :---------------------------------------------------------------------|
| `SHP_LG_PRE_INVOICE_HEADER_PERIOD_NAME` | `PERIODO`                  | Período de referência (mês/ano)                                       |
| `SHP_LG_PRODUCT_TYPE`             | `PRODUCT`                          | Tipo de produto ajustado (ex: `logistics`)                            |
| Calculado                          | `RENTALS_COMP_COST`                | Custo com veículos alugados em dias úteis                             |
| Calculado                          | `RENTALS_REGULAR_COST`             | Custo total de rotas de veículos alugados                             |
| Calculado                          | `RENT_COEF`                        | Coeficiente aluguel (`RENTALS_COMP_COST / RENTALS_REGULAR_COST`)      |
| Calculado                          | `COMP_COST`                        | Custo total de pré-faturas complementares                             |
| Calculado                          | `REG_COST`                         | Custo total de pré-faturas regulares                                  |
| Calculado                          | `COEF`                             | Coeficiente geral (`COMP_COST / REG_COST`)                            |
| Calculado                          | `TOTAL_COST`                       | Custo total do período (complementar + regular)                       |

---

🔁 **Joins e Multiplicadores:**

Joins: *Executados entre `DETAIL`, `ROUTES` e `HEADER` para consolidar custos por período.*

Multiplicadores: *Os coeficientes resultam da divisão entre custos complementares e regulares.*

---

📋 **Regras de Negócio Implícitas:**

- `PRODUCT` é substituído de `place` para `logistics`.
- Apenas registros com valores positivos e fora de cancelamentos são considerados.
- Filtra apenas `step_type = 'last_mile'` e `product_type <> 'crowd'`.
- Coeficientes são calculados para monitorar custos complementares e regulares no período.

---

🔍 **Considerações técnicas:**

- Criação com `CREATE OR REPLACE TABLE`, sobrescrevendo `STG.LK_BASE_COEFICIENTE_COMPLEMENTAR_CUSTOS`.
- Aplica agregações por período, site e produto.
- O `LIMIT 1 ORDER BY PERIODO DESC` garante trazer apenas o último período disponível.

---

⚠️ **Pontos de atenção:**

- A ausência de registros em `REG` ou `COMP` impacta os coeficientes.

---
