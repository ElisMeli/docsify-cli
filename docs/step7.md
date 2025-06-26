### Step 07 – XPT Rotas LM

🔹 Step 07 – `LK_XPT_ROTAS_LM_CUSTOS`

✅ **Objetivo:**

Identificar para cada rota o destino XPT (fora do Brasil) ou NEX (nacional) com base no `SHP_LG_DESTINATION_FACILITY_ID`, permitindo categorizações geográficas no fluxo logístico.

---

⚙️ **Fonte de dados:**

**Tabela utilizada:**
- `meli-bi-data.WHOWNER.BT_SHP_LG_SHIPMENT_CHECKPOINTS`

**Filtros aplicados:**
- `SHP_LG_SHIPMENT_CHK_DT BETWEEN '2025-01-01' AND '2025-12-31'`
- `SHP_LG_ROUTE_ID IS NOT NULL`
- `SHP_LG_DESTINATION_FACILITY_ID IS NOT NULL OU STARTS_WITH(...)`

---

📐 **Transformações e Seleções:**

| *Coluna no output*         | *Descrição*                                                                 |
| :------------------------- | :-------------------------------------------------------------------------- |
| `SHP_LG_ROUTE_ID`          | Identificador da rota logística                                             |
| `SHP_LG_FACILITY_ID`       | Facility de origem vinculada à rota                                         |
| `XPT`                      | Destino internacional (quando `DESTINATION_FACILITY_ID` **não começa com BR**) |
| `NEX`                      | Destino nacional (quando `DESTINATION_FACILITY_ID` **começa com BR**)        |

---

🔁 **Joins e Multiplicadores:**

Joins: *Não há joins aplicados neste step. Os dados são obtidos diretamente da tabela de checkpoints.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- A lógica de separação entre `XPT` e `NEX` depende da análise da string `SHP_LG_DESTINATION_FACILITY_ID`.
- Se o ID começar com `'BR'`, é classificado como `NEX`; caso contrário, como `XPT`.

---

🔍 **Considerações técnicas:**

- A criação da tabela é feita com `CREATE OR REPLACE TABLE`, sobrescrevendo `STG.LK_XPT_ROTAS_LM_CUSTOS`.
- O agrupamento final é feito por `SHP_LG_ROUTE_ID`, `SHP_LG_FACILITY_ID` e `SHP_LG_DESTINATION_FACILITY_ID`.

---

⚠️ **Pontos de atenção:**

- Caso o campo `SHP_LG_DESTINATION_FACILITY_ID` esteja em branco, o registro será descartado.
- Um mesmo `SHP_LG_ROUTE_ID` pode ter múltiplas ocorrências, dependendo da granularidade do destino.
- Essa base é sensível a inconsistências no preenchimento do código da unidade de destino.
