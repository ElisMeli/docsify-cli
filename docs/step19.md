### Step 19 – Status Shipment Routes

🔹 Step 19 – `LK_STATUS_SHIPMENT_ROUTES_CUSTOS`

✅ **Objetivo:**

Mapear os diferentes status dos pacotes (`SHIPMENT_ID`) por rota (`SHP_LG_ROUTE_ID`), permitindo análises operacionais e de qualidade com base nos desfechos logísticos. A base categoriza entregas, falhas, exceções e outros eventos relevantes.

---

⚙️ **Fonte de dados:**

**Tabela utilizada:**
- `meli-bi-data.WHOWNER.BT_SHP_LG_SHIPMENTS_ROUTES` (`LOGS`)

**Filtros aplicados:**
- `LOGS.SIT_SITE_ID = 'MLB'`
- `LOGS.SHP_LG_ROUTE_STATUS = 'close'`
- `LOGS.SHP_LG_ROUTE_INIT_DATE BETWEEN '2025-01-01' AND '2025-12-31'`

---

📐 **Transformações e Seleções:**

| *Coluna no output*     | *Descrição*                                                        |
| :----------------------| :----------------------------------------------------------------- |
| `SHP_LG_ROUTE_ID`      | Identificador da rota                                              |
| `Delivered`            | Total de pacotes entregues (`delivered` e `delivered_place`)       |
| `Missing`              | Total de pacotes com status `missing`                              |
| `Transferred`          | Total de pacotes com status `transferred`                          |
| `INS_*`                | Total por tipo de insucesso logístico (ex: `INS_buyer_absent`)     |
| `stts_Null`            | Total de pacotes com status nulo                                   |
| `Other`                | Total de pacotes com status fora dos padrões definidos             |

> ⚙️ Os campos `INS_` representam tipos específicos de falha na entrega.

---

🔁 **Joins e Multiplicadores:**

Joins: *Não há joins neste step. A agregação é realizada diretamente sobre a base de rotas e pacotes.*

Multiplicadores: *Nenhum multiplicador aplicado neste step.*

---

📋 **Regras de Negócio Implícitas:**

- A categorização dos status segue a taxonomia oficial do `SHP_LG_SHIPMENT_SUB_STATUS`.
- Status que não estão na lista de categorias conhecidas são agrupados como `Other`.
- O agrupamento considera apenas rotas fechadas (`shp_lg_route_status = 'close'`).

---

🔍 **Considerações técnicas:**

- A tabela `STG.LK_STATUS_SHIPMENT_ROUTES_CUSTOS` é criada com `CREATE OR REPLACE TABLE`, sobrescrevendo a anterior.
- Os valores são agregados por `SHP_LG_ROUTE_ID`, mantendo granularidade por rota.

---

⚠️ **Pontos de atenção:**

- Pacotes com `NULL` no status são contabilizados separadamente (`stts_Null`).
- A soma das colunas pode não bater com o total de pacotes se existirem duplicações ou pacotes fora da janela de corte.
- Recomenda-se revisar periodicamente os status mapeados para incorporar novos tipos, se surgirem.
