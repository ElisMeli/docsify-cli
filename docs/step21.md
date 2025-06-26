### Step 21 – Base Final de Rotas LM

🔹 Step 21 – `LK_BASEROTASLM_CUSTOS`

✅ **Objetivo:**

Consolidar todas as informações operacionais, logísticas, financeiras e de performance das rotas Last Mile e MeliOne, em uma base única e estruturada por `ROUTE_ID`. Esta base é utilizada como ponto final para análises e painéis de custos, produtividade e desdobramentos de KPI.

---

⚙️ **Fontes de dados principais:**

- `BT_SHP_LG_SHIPMENTS_ROUTES` (LOGS)
- `BT_SHP_LG_SHIPMENTS`, `LK_SHP_COMPANIES`
- Bases auxiliares de litragens, cluster, região, rateio, modal, custo por rota, status, ciclo, buffer e orçamento

**Filtros aplicados:**
- `LOGS.sit_site_id = 'MLB'`
- `LOGS.shp_lg_route_status = 'close'`
- `LOGS.SHP_LG_ROUTE_INIT_DATE BETWEEN '2025-01-01' AND '2025-12-31'`

---

📐 **Principais colunas e categorias:**

#### 📌 Identificação
- `ROUTE_ID`, `SITE`, `SVC`, `REGIONAL`, `CLUSTER_ID`, `CARRIER_NAME`, `DRIVER_ID`, `PLATE`

#### 🧭 Localização e Hub
- `XPT`, `NEX`, `HUB`

#### 📅 Datas e ciclos
- `DATA_INICIO`, `HORA_INICIO`, `ANO_MES`, `MONTH`, `CICLO_DA_ROTA`, `CICLO_DO_BUDGET`

#### 📦 Operacional e Volume
- `QTDE_PACOTES`, `REAL_SPR`, `DELIVERED`, `DELIVERED_PLACE`, `REENTREGAS`, `DESCONTEINERIZADOS_ONROUTE`, `SHP_GROUP`

#### 📉 Status e Insucessos
- `UNVISITED`, `BUYER_ABSENT`, `BUYER_REJECTED`, `BAD_ADDRESS`, `INACCESSIBLE_ADDRESS`, `STOLEN`, `CLAIMS`, etc.

#### 🚛 Veículo e Modal
- `MODAL`, `CANAL`, `PRODUCT`, `RECEBE_ADICIONAL_PARADA`, `RECEBE_COMPLEMENTAR`

#### 🔢 Métricas técnicas
- `KM_REAL`, `KM_PLANNED`, `LITRAGEM_TOTAL`, `LITRAGEM_MEDIA`

#### 💰 Custo
- `CPR_REAL`, `CPR_FINAL`, `CPS_REAL`

#### 🎯 Orçamento (Budget)
- `SPR_BUDGET`, `CPR_BUDGET`, `CPS_BUDGET`, `ROUTE_TOTAL_BUDGET`, `UNIT_TOTAL_BUDGET`, `COST_VALUE_BUDGET`

---

🔁 **Joins realizados:**

| *Base*                               | *Objetivo*                                  |
| :-----------------------------------| :-------------------------------------------|
| `STG.LK_MODAL_ROTAS_LM_CUSTOS`       | Modal, canal, produto                        |
| `STG.LK_BASE_CUSTO_ROTA_CUSTOS`      | CPR e CPR_FINAL                              |
| `STG.LK_STATUS_SHIPMENT_CUSTOS`      | KPIs de pacotes e insucessos                |
| `STG.LK_BASE_REGIONAIS`              | Regionalização                               |
| `STG.LK_XPT_ROTAS_LM_CUSTOS`         | Identificação de XPT e NEX                   |
| `STG.LK_CLUSTER_ROTAS_LM_CUSTOS`     | Agrupamento de cluster                       |
| `STG.LK_KM_ROTAS_CUSTOS`             | Quilometragem real e planejada               |
| `STG.LK_LITRAGEMROTA_CUSTOS`         | Litragem por rota                            |
| `STG.LK_SHIPMENT_BUFFER_STATUS_CUSTOS` | Flag para pacotes com buffer               |
| `STG.LK_DEPARA_VEICULO_RECEBE_ADICIONAL_PARADA_CUSTOS` | Regras de custo complementar          |
| `STG.LK_BUDGET_ROTAS_LM_CUSTOS`      | Metas de CPR, SPR, CPS por mês/modo/ciclo    |

---

📋 **Regras de Negócio Implícitas:**

- CPR final ajustado por modal e percentual LM.
- Agrupamento de HUBs feito via `STRING_AGG` de origem dos pacotes.
- Cálculo de `CPS_REAL` com divisão segura: `CPR_FINAL / REAL_SPR`.

---

🔍 **Considerações técnicas:**

- A granularidade é única por `ROUTE_ID`.
- Agrupamento e ordenação garantem unicidade.
- Cálculo de `ANO_MES` e `MONTH` via `EXTRACT`.

---

⚠️ **Pontos de atenção:**

- A base depende de múltiplas tabelas intermediárias — todas devem estar atualizadas e consistentes.
- Pacotes sem `REAL_SPR` podem afetar o cálculo de `CPS_REAL`.
- Modais com nomes divergentes ou fora do DE-PARA podem gerar inconsistências nos joins.
