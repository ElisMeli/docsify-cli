### Step 01 - Base Cidade Origem

🔹 Step 01 – `LK_BASE_CIDADE_ORIGEM_CUSTOS`

✅ **Objetivo:**

Criar uma base de referência para mapear as cidades de origem (com estado) de cada `SHP_FACILITY_ID `que pertence ao site `MLB`. Essa base será usada em joins posteriores para associar a origem geográfica dos custos logísticos.

---------------------------------------------------------------------------------------------------

⚙️ **Fonte de dados:**

**Tabela:** `meli-bi-data.WHOWNER.LK_SHP_FACILITIES`

**Descrição:** Contém a base de facilities (Service Centers) do site MLB, com informações de localização (cidade, estado) utilizadas para rastreio `MLB`.

**Filtro aplicado:**
`WHERE SHP_SITE_ID = 'MLB'`

---------------------------------------------------------------------------------------------------

📐 **Transformações e Seleções:**

| :center:**Coluna no Input**: | :center:**Coluna no Output**: | :center:**Descrição**: |
| :-------------------------: | :--------------------------: | :---------------------: |
| `SHP_FACILITY_ID`           | `SHP_FACILITY_ID`            | Identificador do Service Center |
| `SHP_CITY_NAME`             | `CIDADE_ORIGEM`              | Nome da cidade onde está localizado o SVC |
| `SHP_STATE_ID`              | `SHP_STATE_ID`               | Sigla do estado do Service Center |

---------------------------------------------------------------------------------------------------
**🔁 Joins e Multiplicadores:**

Joins: *Não há operações de join neste step. Os dados são extraídos de uma única tabela fonte.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---------------------------------------------------------------------------------------------------

📋 **Regras de Negócio Implícitas:**

A seleção de `SHP_SITE_ID` = `'MLB'` é uma regra de negócio fundamental, garantindo que a análise de custos esteja focada especificamente nas operações de shipping do Brasil.

A renomeação de `SHP_CITY_NAME` para `CIDADE_ORIGEM` reflete a terminologia de negócio e facilita a compreensão do propósito da coluna no contexto de custos de Performance LM.

---------------------------------------------------------------------------------------------------

🔍 **Considerações técnicas:**

A criação é feita com CREATE OR REPLACE TABLE, sobrescrevendo o conteúdo anterior da tabela `STG.LK_BASE_CIDADE_ORIGEM_CUSTOS`.

Nenhuma transformação de dados é aplicada nas colunas — apenas renomeação de `SHP_CITY_NAME` para `CIDADE_ORIGEM`.

---------------------------------------------------------------------------------------------------

⚠️ **Pontos de atenção:**
Essa tabela é dependente da integridade e atualização da `LK_SHP_FACILITIES`.

Está filtrando apenas o site `MLB`, o que limita a abrangência à operação brasileira.


---------------------------------------------------------------------------------------------------