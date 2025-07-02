### Step 05 – Litragem por Rota

🔹 Step 05 – `LK_LITRAGEMROTA_CUSTOS`

✅ **Objetivo:**

Calcular a litragem média e total por rota de entrega Last Mile, considerando a cubagem dos pacotes transportados. Essa base é utilizada para análises de volumetria logística por rota.

---

⚙️ **Fonte de dados:**

**Tabelas utilizadas:**

- `meli-bi-data.WHOWNER.BT_SHP_LG_SHIPMENTS_ROUTES` (`SHPR`)
- `meli-bi-data.WHOWNER.BT_SHP_LG_SHIPMENTS` (`SHP`)
- `meli-bi-data.WHOWNER.LK_SHP_LG_ROUTES` (`LM`)

**Descrição:** Contêm os registros de pacotes, rotas e dimensões necessárias para o cálculo de litragem média e total por rota.

**Filtros aplicados:**

- `SHPR.SIT_SITE_ID = 'MLB'`
- `LM.SHP_LG_TYPE IN ('last_mile', 'melione')`
- `LM.SHP_LG_INIT_DATE BETWEEN '2025-01-01' AND '2025-12-31'`

---

📐 **Transformações e Seleções:**

| **Coluna no Input** | **Coluna no Output** | **Descrição**                                               |
| :-----------------: | :------------------: | :---------------------------------------------------------: |
| `SHP_LG_ROUTE_ID`   | `SHP_LG_ROUTE_ID`    | Identificador da rota logística                             |
| Dimensões do pacote (`HEIGHT`, `WIDTH`, `LENGTH`) | `LITRAGEM_MEDIA` | Litragem média dos pacotes da rota em litros               |
| Dimensões do pacote (`HEIGHT`, `WIDTH`, `LENGTH`) | `LITRAGEM_TOTAL` | Litragem total da rota em litros (soma de todos os pacotes)|

*Cálculo de litragem:*  
A litragem é calculada como:
(ALTURA cm x LARGURA cm x COMPRIMENTO cm) / 1.000.000 * 1000 = litros por pacote.

---

🔁 **Joins e Multiplicadores:**

Joins: *Realizados entre `SHIPMENTS`, `SHIPMENTS_ROUTES` e `ROUTES` para capturar dimensões e contexto das rotas.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- A litragem em litros é calculada a partir das dimensões em centímetros convertidas para metros cúbicos e multiplicadas por 1000.
- São considerados apenas pacotes de rotas com tipo `'last_mile'` e `'melione'`.
- Utiliza-se a data de início da rota para o período de análise de 2025.

---

🔍 **Considerações técnicas:**

- A tabela é criada utilizando `CREATE OR REPLACE`, sobrescrevendo `STG.LK_LITRAGEMROTA_CUSTOS`.
- Utiliza `AVG()` e `SUM()` para cálculo de litragem por `SHP_LG_ROUTE_ID`.
- Considera os campos `SHP_LG_HEIGHT`, `SHP_LG_WIDTH` e `SHP_LG_LENGTH` diretamente dos registros de pacotes.

---

⚠️ **Pontos de atenção:**

- Pacotes sem dimensões preenchidas não entram no cálculo.
- A granularidade permanece por `SHP_LG_ROUTE_ID`, sem detalhamento por data.
- A precisão da litragem depende da qualidade do preenchimento das dimensões na tabela de pacotes (`BT_SHP_LG_SHIPMENTS`).

---
