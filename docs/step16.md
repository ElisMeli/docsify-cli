### Step 16 – Pré Base Custo Rota

🔹 Step 16 – `LK_PRE_BASE_CUSTO_ROTA_CUSTOS`

✅ **Objetivo:**

Construir uma base detalhada por rota, considerando o custo associado à execução, distância percorrida e pacotes movimentados. Esta base é utilizada como pré-processamento antes da consolidação de custos finais por rota.

---

⚙️ **Fonte de dados:**

**Tabelas utilizadas:**
- `BT_SHP_LG_PRE_INVOICE_DETAIL`
- `BT_SHP_LG_PRE_INVOICE_ROUTES`
- `BT_SHP_LG_SHIPMENTS_ROUTES`
- `LK_SHP_COMPANIES`
- `LK_SHP_LG_ROUTES`
- `BT_SHP_SHIPMENTS`
- `LK_SHP_ADDRESS`

**Filtros aplicados:**
- `SIT_SITE_ID = 'MLB'`
- `SHP_LG_ROUTE_INIT_DATE` em 2025
- `SHP_LG_STEP_TYPE` em `'last_mile', 'melione'`
- `SHP_LG_PRE_INVOICE_DETAIL_TYPE_NAME = 'service'`

---

📐 **Transformações e Seleções:**

| **Coluna no Input** | **Coluna no Output** | **Descrição** |
| :------------------: | :------------------: | :----------- |
| `SHP_COMPANY_ID` | `SHP_COMPANY_ID` | ID da empresa contratada |
| `SHP_COMPANY_NAME` | `SHP_COMPANY_NAME` | Nome da empresa |
| `SHP_LG_PRE_INVOICE_HEADER_PERIOD_NAME` | `SHP_LG_PRE_INVOICE_HEADER_PERIOD_NAME` | Período da rota |
| `SHP_LG_PRE_INVOICE_ENTITY_ID` | `ROUTE` | Identificador da rota |
| `SHP_LG_FACILITY_ID` | `SHP_LG_FACILITY_ID` | Unidade logística vinculada |
| `SHP_LG_DRIVER_ID` | `DRIVER` | Identificador do motorista |
| `SHP_LG_STEP_TYPE` | `SHP_LG_STEP_TYPE` | Etapa da rota |
| `SHP_LG_ROUTE_INIT_DATE` | `DATA_INICIO` | Data de início da rota |
| `SHP_LG_PRE_INVOICE_DETAIL_DESCRIPTION` | `SHP_LG_PRE_INVOICE_DETAIL_DESCRIPTION` | Descrição do item de cobrança |
| `SHP_LG_ROUTE_TRAVELED_DISTANCE` | `SHP_LG_ROUTE_TRAVELED_DISTANCE` | Distância real percorrida (m) |
| `SHP_LG_ROUTE_PLANNED_DISTANCE` | `SHP_LG_ROUTE_PLANNED_DISTANCE` | Distância planejada (m) |
| `SHP_LG_PRE_INVOICE_DETAIL_COST` | `COST` | Custo ajustado por pacotes quando aplicável |
| `SHP_SHIPMENT_ID` | `SHP_LG_ROUTE_AMOUNT_SHIPMENTS` | Quantidade de pacotes movimentados |
| `SHP_SHIPMENT_ID` (status `delivered`) | `DELIVERED` | Pacotes entregues |
| `SHP_RECEIVER_ID` | `STOPS` | Quantidade de paradas distintas |

---

🔁 **Joins e Multiplicadores:**

Joins: *Executados entre rotas, faturas, pacotes e endereços para atribuir dimensões financeiras e operacionais.*

Multiplicadores: *Quando o tipo de subitem for `'packages:fixed'`, o custo é multiplicado pelo número de pacotes.*

---

📋 **Regras de Negócio Implícitas:**

- `COST` ajustado com base no tipo de subitem.
- Contagem de `DELIVERED` considera apenas status `'delivered'`.
- `STOPS` calculado pela contagem de `RECEIVER_ID`.

---

🔍 **Considerações técnicas:**

- Tabela criada com `CREATE OR REPLACE`, sobrescrevendo o conteúdo anterior.
- Granularidade por `ROUTE`, detalhada por motorista, período e facility.
- Utilizada como insumo para dashboards de custos por rota.

---

⚠️ **Pontos de atenção:**

- Monitorar a aplicação do multiplicador em custos fixos.

