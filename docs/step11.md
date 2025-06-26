### Step 11 – Budget Rotas

🔹 Step 11 – `LK_BUDGET_ROTAS_LM_CUSTOS`

✅ **Objetivo:**

Importar a base de metas orçamentárias logísticas para 2025, contendo indicadores por `MODAL`, `FACILITY`, ciclo e mês. Essa base permite comparar valores planejados com a execução de rotas Last Mile.

---

⚙️ **Fonte de dados:**

**Tabela utilizada:**
- `meli-bi-data.SBOX_MLB_FORECASTING.MLB_RTG_BUDGET_LM_TARGETS_2025`

**Filtro aplicado:**
- *Nenhum filtro foi aplicado neste step.*

---

📐 **Transformações e Seleções:**

| *Coluna no output*    | *Descrição*                                                             |
| :-------------------- | :---------------------------------------------------------------------- |
| `PERIOD_YEAR_MONTH`   | Período no formato `YYYYMM`                                             |
| `FACILITY_ID`         | Unidade logística (SVC)                                                 |
| `MODAL`               | Modal logístico associado ao budget                                     |
| `CICLO_OPERATIVO`     | Ciclo da operação (ex: semanal, mensal, PM, SD)                         |
| `ROUTE_TOTAL`         | Quantidade total de rotas planejadas no período                         |
| `UNIT_TOTAL`          | Quantidade total de pacotes previstas                                   |
| `COST_VALUE`          | Valor total orçado (budget) em reais para o período e contexto          |
| `SPR_BUDGET`          | Shipments por rota planejado (`SPR`)                                    |
| `CPR_BUDGET`          | Custo por rota planejado (`CPR`)                                        |
| `CPS_BUDGET`          | Custo por shipment planejado (`CPS`)                                    |

---

🔁 **Joins e Multiplicadores:**

Joins: *Não há joins neste step. A base é carregada diretamente com os dados consolidados de planejamento.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- Os valores orçamentários são definidos por combinação de `FACILITY`, `MODAL` e `CICLO_OPERATIVO`.
- Os indicadores `SPR`, `CPR` e `CPS` representam metas derivadas da previsão de pacotes e custo total.

---

🔍 **Considerações técnicas:**

- A criação da tabela é feita com `CREATE OR REPLACE TABLE`, sobrescrevendo a `STG.LK_BUDGET_ROTAS_LM_CUSTOS`.
- Os dados são importados de uma tabela de planejamento, e não há transformações nem filtros aplicados.

---

⚠️ **Pontos de atenção:**

- Certifique-se de que os nomes de MODAL e FACILITY estejam padronizados para que os joins com execuções funcionem corretamente.
- Indicadores como `SPR_BUDGET`, `CPR_BUDGET` e `CPS_BUDGET` são sensíveis à consistência entre pacotes, rotas e valores.
- Ciclos como `PM` e `SD` precisam estar em conformidade com os ciclos atribuídos nas tabelas de execução.
