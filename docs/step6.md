### Step 06 - KM Rotas

🔹 Step 06 – `LK_KM_ROTAS_CUSTOS`

✅ **Objetivo:**

Obter a quilometragem real e planejada percorrida por cada rota de entrega, com base nos dados de pré-faturamento logístico. Essa base é usada para análises de eficiência operacional e comparação entre planejamento e execução.

---

⚙️ **Fonte de dados:**

**Tabela:**  
- `meli-bi-data.WHOWNER.BT_SHP_LG_PRE_INVOICE_DETAIL` (`INV`)  
- `meli-bi-data.WHOWNER.BT_SHP_LG_PRE_INVOICE_ROUTES` (`ROUT`)

**Descrição:** Bases de pré-faturamento que armazenam dados detalhados das rotas e dos custos operacionais, permitindo análise de distâncias percorridas.

**Filtros aplicados:**  
- `INV.SIT_SITE_ID = 'MLB'`  
- `ROUT.SHP_LG_ROUTE_INIT_DATE BETWEEN '2025-01-01' AND '2025-12-31'`  
- `INV.SHP_LG_PRE_INVOICE_DETAIL_TYPE_OPERATION = '+'`

---

📐 **Transformações e Seleções:**

| **Coluna no Input**              | **Coluna no Output** | **Descrição**                                            |
| :------------------------------: | :------------------: | :------------------------------------------------------: |
| `SHP_LG_PRE_INVOICE_ENTITY_ID`   | `ID_ROTA`            | Identificador da rota (pré-faturamento)                 |
| `SHP_LG_ROUTE_TRAVELED_DISTANCE` | `KM_REAL`            | Quilometragem real percorrida pela rota (em km)         |
| `SHP_LG_ROUTE_PLANNED_DISTANCE`  | `KM_PLANNED`         | Quilometragem planejada originalmente (em km)           |

> **Obs:** Distâncias são convertidas de metros para quilômetros com 2 casas decimais.

---

🔁 **Joins e Multiplicadores:**

Joins: *Join entre `PRE_INVOICE_DETAIL` e `PRE_INVOICE_ROUTES` para relacionar rota e distâncias.*  
Multiplicadores: *Não aplicável neste step.*

---

📋 **Regras de Negócio Implícitas:**

- Somente registros com operação `+` são considerados (envios reais, sem cancelamentos).
- Quilometragens convertidas para km para análises operacionais consistentes.

---

🔍 **Considerações técnicas:**

- A tabela é criada com `CREATE OR REPLACE`, sobrescrevendo dados anteriores.
- Granularidade por `ID_ROTA`, com colunas de métricas reais e planejadas.

---

⚠️ **Pontos de atenção:**

- Divergências entre `KM_REAL` e `KM_PLANNED` podem indicar falhas de planejamento ou desvios operacionais.
- Distâncias zeradas ou anômalas devem ser auditadas para garantir a qualidade dos dados utilizados nas análises.

---
