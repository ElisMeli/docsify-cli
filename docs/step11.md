### Step 11 - Budget Rotas

🔹 Step 11 – `LK_BUDGET_ROTAS_LM_CUSTOS`

✅ **Objetivo:**

Importar a base de metas orçamentárias logísticas para 2025, contendo indicadores por `MODAL`, `FACILITY`, ciclo e mês. Essa base permite comparar valores planejados com a execução de rotas Last Mile.

---

⚙️ **Fonte de dados:**

**Tabela utilizada:**
- `meli-bi-data.SBOX_MLB_FORECASTING.MLB_RTG_BUDGET_LM_TARGETS_2025`

**Descrição:** Traz o planejamento logístico orçamentário para o ano de 2025, detalhando volumes, rotas, custos e metas por unidade, modal e ciclo.

**Filtro aplicado:**
- *Nenhum filtro foi aplicado neste step.*

---

📐 **Transformações e Seleções:**

| **Coluna no Input**      | **Coluna no Output**   | **Descrição**                                                    |
| :-----------------------:| :--------------------: | :--------------------------------------------------------------: |
| `PERIOD_YEAR_MONTH`      | `PERIOD_YEAR_MONTH`    | Período no formato `YYYYMM`                                      |
| `FACILITY_ID`            | `SHP_FACILITY_ID`      | Unidade logística (SVC)                                          |
| `MODAL`                  | `MODAL`                | Modal logístico associado ao budget                              |
| `CICLO_OPERATIVO`        | `CICLO_OPERATIVO`      | Ciclo operacional (PM, SD, semanal, etc.)                        |
| `ROUTE_TOTAL`            | `ROUTE_TOTAL`          | Quantidade total de rotas planejadas no período                  |
| `UNIT_TOTAL`             | `UNIT_TOTAL`           | Quantidade total de pacotes previstas                            |
| `COST_VALUE`             | `COST_VALUE`           | Valor total orçado (budget) em reais                             |
| `SPR_2025`               | `SPR_BUDGET`           | Shipments por rota planejado (`SPR`)                             |
| `CPR_2025`               | `CPR_BUDGET`           | Custo por rota planejado (`CPR`)                                 |
| `CPS_2025`               | `CPS_BUDGET`           | Custo por shipment planejado (`CPS`)                             |

---

🔁 **Joins e Multiplicadores:**

Joins: *Não há joins neste step. A base é carregada diretamente com os dados consolidados de planejamento.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- Os valores orçamentários são definidos por combinação de `FACILITY`, `MODAL` e `CICLO_OPERATIVO`.
- Indicadores `SPR`, `CPR` e `CPS` são calculados previamente pela área de planejamento como metas para cada contexto.

---

🔍 **Considerações técnicas:**

- Criação com `CREATE OR REPLACE TABLE`, sobrescrevendo a `STG.LK_BUDGET_ROTAS_LM_CUSTOS`.
- Sem transformações adicionais, garantindo a fidelidade do dado de planejamento.

---

⚠️ **Pontos de atenção:**

- Garanta que os nomes de `MODAL` e `FACILITY` estejam padronizados para que os joins com execuções funcionem corretamente.
- Indicadores `SPR_BUDGET`, `CPR_BUDGET` e `CPS_BUDGET` são sensíveis à consistência entre pacotes, rotas e valores.
- Ciclos como `PM` e `SD` devem estar alinhados ao ciclo atribuído nas tabelas de execução para análises consistentes.

---
