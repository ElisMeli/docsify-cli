### Step 14 – Base Custo Paradas

🔹 Step 14 – `LK_BASE_CUSTO_PARADAS_CUSTOS`

✅ **Objetivo:**

Criar uma base com os custos associados a paradas do tipo `Visita`, por driver, período e unidade logística. O objetivo é mensurar o custo variável relacionado a esse tipo de operação, útil para cálculo de indicadores financeiros por parada ou rota.

---

⚙️ **Fonte de dados:**

**Tabelas utilizadas:**
- `meli-bi-data.WHOWNER.BT_SHP_LG_PRE_INVOICE_DETAIL` (`INV`)
- `meli-bi-data.WHOWNER.BT_SHP_LG_PRE_INVOICE_ROUTES` (`ROUT`)
- `meli-bi-data.WHOWNER.BT_SHP_LG_SHIPMENTS_ROUTES` (`SHP`)
- `meli-bi-data.WHOWNER.LK_SHP_COMPANIES` (`COMP`)
- `meli-bi-data.WHOWNER.LK_SHP_LG_ROUTES` (`XPT`)

**Filtros aplicados:**
- `inv.SIT_SITE_ID = 'MLB'`
- `ROUT.SHP_LG_ROUTE_INIT_DATE BETWEEN '2025-01-01' AND '2025-12-31'`
- `SHP_LG_STEP_TYPE IN ('middle_mile','last_mile','melione')`
- `SHP_LG_PRE_INVOICE_DETAIL_DESCRIPTION` começa com `'Vis'`
- Exclusão de descrições contendo `'INFO: LH'`
- `SHP_LG_PRE_INVOICE_DETAIL_TYPE_OPERATION = '+'`

---

📐 **Transformações e Seleções:**

| *Coluna no output*         | *Descrição*                                                                      |
| :--------------------------| :---------------------------------------------------------------------------------|
| `SHP_COMPANY_ID`           | Identificador da empresa contratada                                              |
| `SHP_COMPANY_NAME`         | Nome da empresa                                                                  |
| `SHP_LG_PRE_INVOICE_HEADER_PERIOD_NAME` | Período de referência do custo                                       |
| `DRIVER`                   | Identificador do motorista responsável pela rota                                 |
| `SHP_LG_FACILITY_ID`       | Unidade logística vinculada à rota                                               |
| `COST`                     | Custo total atribuído à visita (`PRE_INVOICE_DETAIL_COST`)                        |
| `QTY_ROUTES`               | Quantidade de rotas distintas associadas ao driver no período                     |
| `VARIABLE`                 | Custo médio por rota (`COST / QTY_ROUTES`)                                       |

---

🔁 **Joins e Multiplicadores:**

Joins: *Executados entre tabelas de detalhe de fatura, rotas, empresas, shipments e rotas logísticas para coletar as dimensões necessárias.*

Multiplicadores: *Não há aplicação explícita de multiplicadores neste step, mas a divisão por quantidade de rotas resulta em um custo médio.*

---

📋 **Regras de Negócio Implícitas:**

- Apenas custos com descrição iniciando em `'Vis'` são considerados como custo de paradas.
- Custos do tipo `'INFO: LH'` são descartados, por não representarem custo direto de rota.
- A operação considerada é somente do tipo `+`, que representa rotas executadas.

---

🔍 **Considerações técnicas:**

- A granularidade é por `driver`, `empresa`, `período` e `facility`.
- A tabela `STG.LK_BASE_CUSTO_PARADAS_CUSTOS` é criada com `CREATE OR REPLACE TABLE`, sobrescrevendo a anterior.
- A métrica `VARIABLE` representa o custo médio por rota com base nas visitas identificadas.

---

⚠️ **Pontos de atenção:**

- Drivers com múltiplas rotas no mesmo período terão o custo diluído na média.
- A lógica depende fortemente da descrição iniciar com `'Vis'`; inconsistências no preenchimento podem afetar a análise.
- Custos muito baixos ou altos devem ser validados individualmente, especialmente se a contagem de rotas for pequena.
