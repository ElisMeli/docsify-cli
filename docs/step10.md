### Step 10 – Modal Rotas LM

🔹 Step 10 – `LK_MODAL_ROTAS_LM_CUSTOS`

✅ **Objetivo:**

Classificar cada rota com seu respectivo `MODAL`, `CANAL` e `PRODUCT` a partir de um mapeamento de veículo, canal e tipo de produto, com base nas entregas executadas em 2025. Essa base define a estrutura modal da malha logística utilizada.

---

⚙️ **Fonte de dados:**

**Tabelas utilizadas:**
- `meli-bi-data.WHOWNER.BT_SHP_LG_SHIPMENTS_ROUTES` (`LOGS`)
- `meli-bi-data.WHOWNER.LK_SHP_COMPANIES` (`COMP`)
- `meli-bi-data.SBOX_ANALYTICSLASTMILE.DE_PARA_BUDGET_MODAL_2025` (`d`)

**Filtros aplicados:**
- `LOGS.sit_site_id = 'MLB'`
- `LOGS.shp_lg_route_status = 'close'`
- `LOGS.SHP_LG_ROUTE_INIT_DATE BETWEEN '2025-01-01' AND '2025-12-31'`

---

📐 **Transformações e Seleções:**

| *Coluna no output*   | *Descrição*                                                                 |
| :------------------- | :-------------------------------------------------------------------------- |
| `SHP_LG_ROUTE_ID`    | Identificador da rota logística                                              |
| `MODAL`              | Modal logístico atribuído à rota (Ex: Van, Moto, Fixa, etc.)                 |
| `CANAL`              | Canal logístico (Ex: Envios Extra, Kangu Logistics, Agencias, etc.)          |
| `PRODUCT`            | Tipo de produto ou serviço (Ex: Delivery, Coleta, Logística Reversa, etc.)   |

---

🔁 **Joins e Multiplicadores:**

Joins: *Realizados joins entre `SHIPMENTS_ROUTES`, `COMPANIES` e o mapeamento modal (`DE_PARA_BUDGET_MODAL_2025`) para derivar as classificações modal, canal e produto.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- A correspondência entre `SHP_LG_VEHICLE_TYPE` e os campos `DE`, `CANAL` e `PRODUCT` é baseada na lógica do `DE_PARA_BUDGET_MODAL_2025`.
- Para canais `Envios Extra` e `Kangu Logistics`, a associação exige correspondência exata do `shp_company_name`.
- A seleção do registro é feita com `ROW_NUMBER()` para garantir uma única combinação por rota (`RN = 1`).

---

🔍 **Considerações técnicas:**

- A criação da tabela é feita com `CREATE OR REPLACE TABLE`, sobrescrevendo `STG.LK_MODAL_ROTAS_LM_CUSTOS`.
- A granularidade da base é por `SHP_LG_ROUTE_ID`, com as colunas `MODAL`, `CANAL` e `PRODUCT` associadas.

---

⚠️ **Pontos de atenção:**

- É fundamental que o mapeamento da tabela `DE_PARA_BUDGET_MODAL_2025` esteja atualizado para evitar classificações incompletas.
- A lógica condicional dos joins pode deixar algumas rotas sem MODAL caso não haja correspondência nos critérios.
- A deduplicação por `ROW_NUMBER()` assume que há apenas uma linha prioritária por rota — divergências devem ser analisadas.
