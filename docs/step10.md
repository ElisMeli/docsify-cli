### Step 10 - Modal Rotas LM

🔹 Step 10 – `LK_MODAL_ROTAS_LM_CUSTOS`

✅ **Objetivo:**

Classificar cada rota com seu respectivo `MODAL`, `CANAL` e `PRODUCT` a partir de um mapeamento de veículo, canal e tipo de produto, utilizando as entregas executadas em 2025. Essa base define a estrutura modal da malha logística utilizada.

---

⚙️ **Fonte de dados:**

**Tabelas utilizadas:**
- `meli-bi-data.WHOWNER.BT_SHP_LG_SHIPMENTS_ROUTES` (`LOGS`)
- `meli-bi-data.WHOWNER.LK_SHP_COMPANIES` (`COMP`)
- `meli-bi-data.SBOX_ANALYTICSLASTMILE.DE_PARA_BUDGET_MODAL_2025` (`d`)

**Descrição:** Traz informações de execução de rotas, detalhes das empresas associadas e o mapeamento modal atualizado para 2025.

**Filtros aplicados:**
- `LOGS.sit_site_id = 'MLB'`
- `LOGS.shp_lg_route_status = 'close'`
- `LOGS.SHP_LG_ROUTE_INIT_DATE BETWEEN '2025-01-01' AND '2025-12-31'`

---

📐 **Transformações e Seleções:**

| **Coluna no Input** | **Coluna no Output** | **Descrição**                                            |
| :-----------------: | :------------------: | :------------------------------------------------------: |
| `SHP_LG_ROUTE_ID`   | `SHP_LG_ROUTE_ID`    | Identificador da rota logística                          |
| (Derivado de mapeamento) | `MODAL`         | Modal logístico atribuído à rota                         |
| (Derivado de mapeamento) | `CANAL`         | Canal logístico vinculado à operação da rota             |
| (Derivado de mapeamento) | `PRODUCT`       | Tipo de produto ou serviço executado pela rota           |

---

🔁 **Joins e Multiplicadores:**

Joins: *Realizados joins entre `SHIPMENTS_ROUTES`, `COMPANIES` e o mapeamento modal (`DE_PARA_BUDGET_MODAL_2025`) para derivar as classificações de modal, canal e produto.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- O campo `DE` no `DE_PARA_BUDGET_MODAL_2025` é utilizado para mapear o `SHP_LG_VEHICLE_TYPE` e atribuir os campos `MODAL`, `CANAL` e `PRODUCT`.
- Para os canais `Envios Extra` e `Kangu Logistics`, é necessário o match exato no `shp_company_name` para associação.
- Deduplicação realizada via `ROW_NUMBER()` garantindo apenas um registro por rota.

---

🔍 **Considerações técnicas:**

- Criação realizada com `CREATE OR REPLACE TABLE`, sobrescrevendo a `STG.LK_MODAL_ROTAS_LM_CUSTOS`.
- Granularidade por `SHP_LG_ROUTE_ID`, garantindo as colunas `MODAL`, `CANAL` e `PRODUCT` associadas.

---

⚠️ **Pontos de atenção:**

- A base de mapeamento `DE_PARA_BUDGET_MODAL_2025` deve estar atualizada para garantir a classificação correta das rotas.
- Em caso de inconsistências ou ausência de match nos critérios, a rota pode ficar sem `MODAL`.
- A deduplicação via `ROW_NUMBER()` assume que há apenas um match prioritário por rota.

---
