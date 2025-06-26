### Step 13 – Base Coeficiente Complementar

🔹 Step 13 – `LK_BASE_COEFICIENTE_COMPLEMENTAR_CUSTOS`

✅ **Objetivo:**

Construir uma base com coeficientes comparativos entre custos regulares e complementares, além de cálculos específicos para veículos alugados. Essa base consolida indicadores financeiros por período, úteis para ajustes de modelagem orçamentária.

---

⚙️ **Fonte de dados:**

**Tabelas utilizadas:**
- `meli-bi-data.WHOWNER.BT_SHP_LG_PRE_INVOICE_DETAIL`
- `meli-bi-data.WHOWNER.BT_SHP_LG_PRE_INVOICE_ROUTES`
- `meli-bi-data.WHOWNER.BT_SHP_LG_PRE_INVOICE_HEADER`

**Filtros aplicados:**
- `SIT_SITE_ID = 'MLB'`
- `SHP_LG_PRE_INVOICE_HEADER_STEP_TYPE = 'last_mile'`
- `SHP_LG_PRE_INVOICE_HEADER_STATUS_NAME NOT IN ('canceled')`
- `SHP_LG_PRODUCT_TYPE NOT IN ('crowd')`
- `SHP_LG_PRE_INVOICE_HEADER_TOTAL_COST > 0`

---

📐 **Transformações e Seleções:**

| *Coluna no output*         | *Descrição*                                                                 |
| :--------------------------| :--------------------------------------------------------------------------- |
| `PERIODO`                  | Período de referência (`SHP_LG_PRE_INVOICE_HEADER_PERIOD_NAME`)              |
| `PRODUCT`                  | Tipo de produto ajustado (ex: `logistics`)                                   |
| `RENTALS_COMP_COST`        | Custo com veículos alugados em dias úteis (`Rentals Costs - WORKING DAY`)    |
| `RENTALS_REGULAR_COST`     | Custo total das rotas de veículos alugados com tipos específicos             |
| `RENT_COEF`                | Coeficiente de aluguel (`RENTALS_COMP_COST / RENTALS_REGULAR_COST`)          |
| `COMP_COST`                | Custo total de pré-faturas complementares no período                         |
| `REG_COST`                 | Custo total de pré-faturas regulares no período                              |
| `COEF`                     | Coeficiente geral (`COMP_COST / REG_COST`)                                   |
| `TOTAL_COST`               | Soma total de custos no período (complementar + regular)                     |

---

🔁 **Joins e Multiplicadores:**

Joins: *Executados para cruzar os custos por período entre pré-faturas (`HEADER`), detalhes (`DETAIL`) e rotas (`ROUTES`).*

Multiplicadores: *Não há aplicação explícita de multiplicadores neste step, mas os coeficientes são frações entre custos complementares e regulares.*

---

📋 **Regras de Negócio Implícitas:**

- Custo `complementar` e `regular` são segregados com base no campo `SHP_LG_PRE_INVOICE_HEADER_TYPE`.
- O campo `PRODUCT` é ajustado: se `place`, então substituído por `logistics`.
- Apenas registros com custo total positivo e fora de cancelamento são considerados.
- São consideradas apenas linhas do step `last_mile` e produtos não `crowd`.

---

🔍 **Considerações técnicas:**

- A base aplica agregações por período, produto e site.
- O cálculo final usa `LIMIT 1 ORDER BY PERIODO DESC` para trazer apenas o registro mais recente.
- A tabela `STG.LK_BASE_COEFICIENTE_COMPLEMENTAR_CUSTOS` é sobrescrita a cada execução com `CREATE OR REPLACE`.

---

⚠️ **Pontos de atenção:**

- O uso de `LIMIT 1` retorna apenas o último período — se o objetivo for série histórica, essa limitação deve ser removida.
- Os cálculos de coeficiente são sensíveis à ausência de registros em qualquer uma das partes (REG ou COMP).
- Verifique se todos os tipos de veículos alugados estão atualizados no filtro do bloco `REGRENT`.
