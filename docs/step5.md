### Step 03 - Litragem por Rota

🔹 Step 03 – `LK_LITRAGEMROTA_CUSTOS`

✅ **Objetivo:**

Calcular a litragem média e total por rota de entrega Last Mile, considerando a cubagem dos pacotes transportados. Essa base será utilizada para análises relacionadas à volumetria logística por rota.

---

⚙️ **Fonte de dados:**

**📄 Tabelas utilizadas:**
- `meli-bi-data.WHOWNER.BT_SHP_LG_SHIPMENTS_ROUTES` (`SHPR`)
- `meli-bi-data.WHOWNER.BT_SHP_LG_SHIPMENTS` (`SHP`)
- `meli-bi-data.WHOWNER.LK_SHP_LG_ROUTES` (`LM`)

**🔎 Filtros aplicados:**
- `SIT_SITE_ID = 'MLB'`
- `SHP_LG_TYPE IN ('last_mile', 'melione')`
- `SHP_LG_INIT_DATE BETWEEN '2025-01-01' AND '2025-12-31'`

---

📐 **Transformações e Seleções:**

| **Coluna no output**   | **Descrição**                                                                 |
| :----------------------| :----------------------------------------------------------------------------- |
| `SHP_LG_ROUTE_ID`      | Identificador da rota logística                                                |
| `LITRAGEM_MEDIA`       | Litragem média dos pacotes da rota (média da cubagem total / número de pacotes) |
| `LITRAGEM_TOTAL`       | Litragem total da rota (soma das cubagens em litros de todos os pacotes)       |

*Cálculo de litragem:*  
Altura × Largura × Comprimento (em metros cúbicos) × 1000 = litros

---

🔁 **Joins e Multiplicadores:**

Joins: *Realizados joins entre SHIPMENTS, ROUTES e ROTAS para capturar os dados de dimensão e contexto da rota.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

A cubagem em litros é calculada com base nas dimensões em centímetros, convertidas para metros cúbicos e multiplicadas por 1000.

O cálculo considera apenas rotas do tipo `'last_mile'` e `'melione'`, garantindo foco na malha final de entrega.

---

🔍 **Considerações técnicas:**

A tabela é criada com `CREATE OR REPLACE`, substituindo o conteúdo anterior.

A agregação é feita por `SHP_LG_ROUTE_ID`, com `AVG()` e `SUM()` da litragem por rota.

As colunas `SHP_LG_HEIGHT`, `WIDTH` e `LENGTH` são consideradas com base nos dados brutos de cada pacote.

---

⚠️ **Pontos de atenção:**

- Se houver pacotes com dimensões faltantes ou nulas, eles não são considerados no cálculo.
- A granularidade da tabela é por `SHP_LG_ROUTE_ID`, sem considerar a data diretamente.
- A precisão dos dados depende da qualidade do preenchimento de medidas na tabela de shipments.
