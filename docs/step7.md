### Step 07 - XPT Rotas LM

🔹 Step 07 – `LK_XPT_ROTAS_LM_CUSTOS`

✅ **Objetivo:**

Identificar para cada rota o destino XPT ou NEX com base no `SHP_LG_DESTINATION_FACILITY_ID`, permitindo categorizações geográficas no fluxo logístico.

---

⚙️ **Fonte de dados:**

**Tabela utilizada:**
- `meli-bi-data.WHOWNER.BT_SHP_LG_SHIPMENT_CHECKPOINTS`

**Descrição:** Contém checkpoints de pacotes com informações de facility de destino utilizadas para segmentação XPT/NEX.

**Filtros aplicados:**
- `SHP_LG_SHIPMENT_CHK_DT BETWEEN '2025-01-01' AND '2025-12-31'`
- `SHP_LG_ROUTE_ID IS NOT NULL`
- `SHP_LG_DESTINATION_FACILITY_ID IS NOT NULL OU STARTS_WITH(...)`

---

📐 **Transformações e Seleções:**

| **Coluna no Input**             | **Coluna no Output** | **Descrição**                                                                  |
| :-----------------------------: | :------------------: | :----------------------------------------------------------------------------- |
| `SHP_LG_ROUTE_ID`               | `SHP_LG_ROUTE_ID`   | Identificador da rota logística                                               |
| `SHP_LG_FACILITY_ID`            | `SHP_FACILITY_ID`   | Facility de origem vinculada à rota                                           |
| `SHP_LG_DESTINATION_FACILITY_ID`| `XPT` ou `NEX`      | Classificação como destino (XPT) ou(NEX) conforme ID  |

---

🔁 **Joins e Multiplicadores:**

Joins: *Não há joins aplicados neste step. Os dados são obtidos diretamente da tabela de checkpoints.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- Se `SHP_LG_DESTINATION_FACILITY_ID` começar com `'BR'`, é classificado como `NEX`.
- Caso contrário, é classificado como `XPT`.
- Essa separação facilita a segmentação de rotas por tipo de destino.

---

🔍 **Considerações técnicas:**

- Criação com `CREATE OR REPLACE TABLE`, sobrescrevendo a `STG.LK_XPT_ROTAS_LM_CUSTOS`.
- O agrupamento é feito por `SHP_LG_ROUTE_ID`, `SHP_LG_FACILITY_ID`, `SHP_LG_DESTINATION_FACILITY_ID`.

---

⚠️ **Pontos de atenção:**

- Registros com `SHP_LG_DESTINATION_FACILITY_ID` em branco são descartados.
- Um mesmo `SHP_LG_ROUTE_ID` pode ter múltiplas ocorrências dependendo do destino.
- A qualidade desta base depende do correto preenchimento dos campos de destino.

---
