### Step 16 – Pré Base Custo Rota

🔹 Step 16 – `LK_PRE_BASE_CUSTO_ROTA_CUSTOS`

✅ **Objetivo:**

Construir uma base detalhada por rota, considerando o custo associado à execução, distância percorrida e pacotes movimentados. Esta base é utilizada como pré-processamento antes da consolidação de custos finais por rota.

---

⚙️ **Fonte de dados:**

**Tabelas utilizadas:**
- `BT_SHP_LG_PRE_INVOICE_DETAIL` (`INV`)
- `BT_SHP_LG_PRE_INVOICE_ROUTES` (`ROUT`)
- `BT_SHP_LG_SHIPMENTS_ROUTES` (`SHP`)
- `LK_SHP_COMPANIES` (`COMP`)
- `LK_SHP_LG_ROUTES` (`XPT`)
- `BT_SHP_SHIPMENTS` (`BSHP`)
- `LK_SHP_ADDRESS` (`ADDR`)

**Filtros aplicados:**
- `INV.SIT_SITE_ID = 'MLB'`
- `ROUT.SHP_LG_ROUTE_INIT_DATE BETWEEN '2025-01-01' AND '2025-12-31'`
- `SHP_LG_STEP_TYPE IN ('last_mile', 'melione')`
- `SHP_LG_PRE_INVOICE_DETAIL_TYPE_NAME = 'service'`

---

📐 **Transformações e Seleções:**

| *Coluna no output*         | *Descrição*                                                                 |
| :--------------------------| :-------------------------------------------------------------------------- |
| `SHP_COMPANY_ID`           | ID da empresa contratada                                                    |
| `SHP_COMPANY_NAME`         | Nome da empresa                                                             |
| `SHP_LG_PRE_INVOICE_HEADER_PERIOD_NAME` | Período da rota                                               |
| `ROUTE`                    | Identificador da rota                                                       |
| `SHP_LG_FACILITY_ID`       | Unidade logística vinculada à rota                                          |
| `DRIVER`                   | Identificador do motorista                                                  |
| `SHP_LG_STEP_TYPE`         | Etapa da rota (Last Mile ou MeliOne)                                        |
| `DATA_INICIO`              | Data de início da rota                                                      |
| `SHP_LG_PRE_INVOICE_DETAIL_DESCRIPTION` | Descrição do item de cobrança                             |
| `SHP_LG_ROUTE_TRAVELED_DISTANCE` | Distância real percorrida (em metros)                            |
| `SHP_LG_ROUTE_PLANNED_DISTANCE` | Distância planejada para a rota (em metros)                       |
| `COST`                     | Custo da rota ajustado (multiplicado pela quantidade de pacotes, se fixo)   |
| `SHP_LG_ROUTE_AMOUNT_SHIPMENTS` | Quantidade total de pacotes da rota                              |
| `DELIVERED`                | Quantidade de pacotes entregues com sucesso                                 |
| `STOPS`                    | Quantidade de paradas distintas (com base no `RECEIVER_ID`)                 |

---

🔁 **Joins e Multiplicadores:**

Joins: *Executados entre as rotas, faturamentos, empresas, endereços e pacotes para agregar todas as informações necessárias por rota.*

Multiplicadores: *Se o tipo de subitem for `'packages:fixed'`, o custo é multiplicado pela quantidade de pacotes.*

---

📋 **Regras de Negócio Implícitas:**

- O campo `COST` depende da lógica condicional baseada em `SHP_LG_PRE_INVOICE_DETAIL_SUB_TYPE`.
- A contagem de entregas (`DELIVERED`) considera apenas pacotes com status `'delivered'`.
- A quantidade de paradas é derivada da contagem de `RECEIVER_ID`.

---

🔍 **Considerações técnicas:**

- A tabela `STG.LK_PRE_BASE_CUSTO_ROTA_CUSTOS` é criada com `CREATE OR REPLACE`, sobrescrevendo seu conteúdo anterior.
- A granularidade é por `ROUTE` (rota executada), com detalhamento por motorista, unidade e período.

---

⚠️ **Pontos de atenção:**

- A multiplicação do custo por pacotes deve ser validada apenas para subtipos fixos.
- Divergências entre pacotes movimentados e entregues podem indicar falhas operacionais.
- A definição correta de paradas depende da integridade do campo `RECEIVER_ID` na base de endereços.
