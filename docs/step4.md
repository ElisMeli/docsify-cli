### Step 04 – Cluster Rotas LM

🔹 Step 04 – `LK_CLUSTER_ROTAS_LM_CUSTOS`

✅ **Objetivo:**

Mapear o principal `CLUSTER_ID` associado a cada rota (`SHP_LG_ROUTE_ID`), com base na frequência de pacotes vinculados. Essa base consolida a associação dominante entre rotas e agrupamentos logísticos (clusters), úteis para análises operacionais e regionais.

---

⚙️ **Fonte de dados:**

**Tabelas utilizadas:**
- `meli-bi-data.WHOWNER.LK_SHP_LG_ROUTES`
- `meli-bi-data.WHOWNER.BT_SHP_LG_SHIPMENTS_ROUTES`
- `meli-bi-data.WHOWNER.BT_SHP_LG_SHIPMENTS`

**Filtro aplicado:**

- `SHPR.SHP_LG_SHIPMENT_SUB_STATUS IS NOT NULL`
- `SHPR.SIT_SITE_ID = 'MLB'`
- `LM.SHP_LG_INIT_DATE BETWEEN '2025-01-01' AND '2025-12-31'`
- `LM.SHP_LG_ROUTE_STATUS = 'close'`

---

📐 **Transformações e Seleções:**

| *Coluna no output* | *Descrição* |
| :----------------- | :---------- |
| `ID_ROTA_REAL`     | Identificador da rota logística (`SHP_LG_ROUTE_ID`) |
| `CLUSTER_ID`       | Código do cluster mais frequente associado à rota |

---

🔁 **Joins e Multiplicadores:**

Joins: *Realizados joins entre `ROUTES`, `SHIPMENTS_ROUTES` e `SHIPMENTS` para capturar os dados de cluster associados a cada pacote da rota.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- O `CLUSTER_ID` é determinado com base na maior contagem de pacotes por rota.
- Quando `SHP_LG_CLUSTER_ID` está vazio ou nulo, o valor completo é mantido como identificador.
- Em caso de empate entre clusters, o critério de desempate é o `CLUSTER_ID` em ordem alfabética.

---

🔍 **Considerações técnicas:**

- A criação é feita com `CREATE OR REPLACE TABLE`, sobrescrevendo a tabela `STG.LK_CLUSTER_ROTAS_LM_CUSTOS`.
- A lógica de agregação utiliza `ROW_NUMBER()` com particionamento por rota e ordenação por volume de pacotes (`SOMA_PCTS DESC`).

---

⚠️ **Pontos de atenção:**

- A granularidade é por `SHP_LG_ROUTE_ID`, ou seja, um cluster por rota.
- Resultados dependem da completude do campo `SHP_LG_CLUSTER_ID` nos pacotes associados.
