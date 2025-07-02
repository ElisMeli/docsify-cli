### Step 14 - Base Custo Paradas

🔹 Step 14 – `LK_BASE_CUSTO_PARADAS_CUSTOS`

✅ **Objetivo:**

Criar uma base com os custos associados a paradas do tipo `Visita`, por driver, período e unidade logística, mensurando o custo variável relacionado a esse tipo de operação, útil para cálculo de indicadores financeiros por parada ou rota.

---

⚙️ **Fonte de dados:**

**Tabelas utilizadas:**
- `meli-bi-data.WHOWNER.BT_SHP_LG_PRE_INVOICE_DETAIL` (`INV`)
- `meli-bi-data.WHOWNER.BT_SHP_LG_PRE_INVOICE_ROUTES` (`ROUT`)
- `meli-bi-data.WHOWNER.BT_SHP_LG_SHIPMENTS_ROUTES` (`SHP`)
- `meli-bi-data.WHOWNER.LK_SHP_COMPANIES` (`COMP`)
- `meli-bi-data.WHOWNER.LK_SHP_LG_ROUTES` (`XPT`)

**Descrição:** Tabelas de pré-faturamento, rotas e shipments utilizadas para capturar custos logísticos detalhados por driver e período.

**Filtros aplicados:**
- `inv.SIT_SITE_ID = 'MLB'`
- `ROUT.SHP_LG_ROUTE_INIT_DATE BETWEEN '2025-01-01' AND '2025-12-31'`
- `SHP_LG_STEP_TYPE IN ('middle_mile','last_mile','melione')`
- Descrição de custo iniciando com `'Vis'`
- Exclusão de descrições contendo `'INFO: LH'`
- Tipo de operação `= '+'` (rotas executadas)

---

📐 **Transformações e Seleções:**

| **Coluna no Input**                   | **Coluna no Output**                | **Descrição**                                                     |
| :------------------------------------:| :----------------------------------:| :---------------------------------------------------------------- |
| `ROUT.SHP_COMPANY_ID`                 | `SHP_COMPANY_ID`                    | Identificador da empresa contratada                               |
| `COMP.SHP_COMPANY_NAME`               | `SHP_COMPANY_NAME`                  | Nome da empresa contratada                                        |
| `ROUT.SHP_LG_PRE_INVOICE_HEADER_PERIOD_NAME` | `PERIODO`                   | Período de referência do custo                                    |
| `SHP.SHP_LG_DRIVER_ID`                | `DRIVER`                            | Identificador do motorista responsável                            |
| `SHP.SHP_LG_FACILITY_ID`              | `SHP_FACILITY_ID`                   | Unidade logística vinculada à rota                                |
| `INV.SHP_LG_PRE_INVOICE_DETAIL_COST`  | `COST`                              | Custo total atribuído à visita                                    |
| Calculado                             | `QTY_ROUTES`                        | Quantidade de rotas distintas associadas ao driver no período     |
| Calculado                             | `VARIABLE`                          | Custo médio por rota (`COST / QTY_ROUTES`)                        |

---

🔁 **Joins e Multiplicadores:**

Joins: *Executados entre tabelas de pré-faturamento, rotas, shipments, empresas e rotas logísticas para coletar todas as dimensões necessárias.*

Multiplicadores: *Não há aplicação explícita, mas a métrica `VARIABLE` resulta da divisão de custo total pelo número de rotas.*

---

📋 **Regras de Negócio Implícitas:**

- Apenas custos com descrição iniciando em `'Vis'` são considerados como custos de parada.
- Custos contendo `'INFO: LH'` são desconsiderados por não representarem paradas.
- Apenas operações executadas (`'+'`) são consideradas.
- Métrica `VARIABLE` calcula o custo médio por rota.

---

🔍 **Considerações técnicas:**

- Criação com `CREATE OR REPLACE TABLE`, sobrescrevendo `STG.LK_BASE_CUSTO_PARADAS_CUSTOS`.
- Granularidade: driver, empresa, período e unidade logística.
- Indicador `VARIABLE` facilita comparações entre custos e rotas realizadas.

---

⚠️ **Pontos de atenção:**

- Drivers com múltiplas rotas no mesmo período terão custos diluídos na média.
- Descrições inconsistentes (não iniciando com `'Vis'`) podem causar exclusões.
- Custos unitários muito baixos ou altos devem ser analisados caso a quantidade de rotas seja baixa.

---
