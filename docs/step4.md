### Step 04 – Cluster Rotas LM

🔹 Step 04 – `LK_CLUSTER_ROTAS_LM_CUSTOS`

✅ **Objetivo:**

Mapear o principal `CLUSTER_ID` associado a cada rota (`SHP_LG_ROUTE_ID`), com base na frequência de pacotes vinculados, consolidando a associação dominante entre rotas e agrupamentos logísticos (clusters) para análises operacionais e regionais.

---

⚙️ **Fonte de dados:**

**Tabelas utilizadas:**

- `meli-bi-data.WHOWNER.LK_SHP_LG_ROUTES`
- `meli-bi-data.WHOWNER.BT_SHP_LG_SHIPMENTS_ROUTES`
- `meli-bi-data.WHOWNER.BT_SHP_LG_SHIPMENTS`

**Descrição:** Contêm os dados de rotas, pacotes vinculados e clusters logísticos utilizados para identificar o cluster dominante por rota.

**Filtro aplicado:**

- `SHPR.SHP_LG_SHIPMENT_SUB_STATUS IS NOT NULL`
- `SHPR.SIT_SITE_ID = 'MLB'`
- `LM.SHP_LG_INIT_DATE BETWEEN '2025-01-01' AND '2025-12-31'`
- `LM.SHP_LG_ROUTE_STATUS = 'close'`

---

📐 **Transformações e Seleções:**

| **Coluna no Input** | **Coluna no Output** | **Descrição**                                           |
| :-----------------: | :------------------: | :-----------------------------------------------------: |
| `SHP_LG_ROUTE_ID`   | `ID_ROTA_REAL`       | Identificador da rota logística                         |
| `SHP_LG_CLUSTER_ID` | `CLUSTER_ID`         | Cluster mais frequente vinculado à rota                |

---

🔁 **Joins e Multiplicadores:**

Joins: *Realizados entre `LK_SHP_LG_ROUTES`, `BT_SHP_LG_SHIPMENTS_ROUTES` e `BT_SHP_LG_SHIPMENTS` para coletar os clusters vinculados a cada pacote das rotas.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- O `CLUSTER_ID` é atribuído considerando a maior quantidade de pacotes vinculados à rota.
- Se `SHP_LG_CLUSTER_ID` estiver vazio ou nulo, mantém-se o valor completo.
- Em caso de empate, considera-se o `CLUSTER_ID` em ordem alfabética.

---

🔍 **Considerações técnicas:**

- Criação realizada com `CREATE OR REPLACE TABLE`, sobrescrevendo a `STG.LK_CLUSTER_ROTAS_LM_CUSTOS`.
- Utilização de `ROW_NUMBER()` para identificar o cluster dominante por rota com `SOMA_PCTS DESC`.

---

⚠️ **Pontos de atenção:**

- A granularidade é por `SHP_LG_ROUTE_ID`, mantendo apenas um `CLUSTER_ID` por rota.
- Os resultados dependem da qualidade e completude do campo `SHP_LG_CLUSTER_ID` nos pacotes vinculados.
- Alterações na estrutura de clusters impactam as análises regionais e os steps seguintes.

---
