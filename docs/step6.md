### Step 06 – KM Rotas

🔹 Step 06 – `LK_KM_ROTAS_CUSTOS`

✅ **Objetivo:**

Obter a quilometragem real e planejada percorrida por cada rota de entrega, com base nos dados de pré-faturamento logístico. Essa base é usada para análises de eficiência operacional e comparação entre planejamento e execução.

---

⚙️ **Fonte de dados:**

**Tabelas utilizadas:**
- `meli-bi-data.WHOWNER.BT_SHP_LG_PRE_INVOICE_DETAIL` (`INV`)
- `meli-bi-data.WHOWNER.BT_SHP_LG_PRE_INVOICE_ROUTES` (`ROUT`)

**Filtros aplicados:**
- `INV.SIT_SITE_ID = 'MLB'`
- `ROUT.SHP_LG_ROUTE_INIT_DATE BETWEEN '2025-01-01' AND '2025-12-31'`
- `INV.SHP_LG_PRE_INVOICE_DETAIL_TYPE_OPERATION = '+'`

---

📐 **Transformações e Seleções:**

| *Coluna no output* | *Descrição*                                                        |
| :----------------- | :----------------------------------------------------------------- |
| `ID_ROTA`          | Identificador da rota (ID de pré-faturamento da entidade logística) |
| `KM_REAL`          | Quilometragem real percorrida pela rota (convertida para km)        |
| `KM_PLANNED`       | Quilometragem planejada originalmente para a rota (convertida para km) |

---

🔁 **Joins e Multiplicadores:**

Joins: *Realizado join entre `PRE_INVOICE_DETAIL` e `PRE_INVOICE_ROUTES` para relacionar o identificador da rota com as métricas de distância.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- Distâncias são convertidas de metros para quilômetros com duas casas decimais.
- Apenas registros com operação do tipo `+` são considerados (envio e execução, não cancelamentos).

---

🔍 **Considerações técnicas:**

- A criação da tabela é feita com `CREATE OR REPLACE TABLE`, sobrescrevendo a `STG.LK_KM_ROTAS_CUSTOS`.
- A base possui granularidade por `ID_ROTA`, com duas métricas numéricas associadas.

---

⚠️ **Pontos de atenção:**

- Diferenças significativas entre `KM_PLANNED` e `KM_REAL` podem indicar falhas na roteirização ou desvios operacionais.
- Distâncias muito altas ou zeradas devem ser avaliadas para garantir consistência dos dados de entrada.
