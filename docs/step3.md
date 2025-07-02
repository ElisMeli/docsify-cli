### Step 03 – Base Regionais

🔹 Step 03 – `LK_BASE_REGIONAIS`

✅ **Objetivo:**

Criar uma base de apoio para mapear a relação entre unidades logísticas e suas respectivas regionais operacionais, possibilitando segmentações por `REGIONAL` nas análises de Performance de Custos LM.

---

⚙️ **Fonte de dados:**

**Tabela:** `meli-bi-data.SBOX_CASECENTERMLB.NOMINAL_REGIONALOTR_IMPORTRANGE`

**Descrição:** Contém mapeamento manual das unidades logísticas (`SIGLA_LG_FACILITY`) e suas respectivas regionais (`OTR_REGIONAL`), utilizado para segmentação operacional nas análises.

**Filtro aplicado:**

*Nenhum filtro aplicado neste step.*

---

📐 **Transformações e Seleções:**

| **Coluna no Input**      | **Coluna no Output** | **Descrição**                                    |
| :-----------------------:| :-------------------:| :---------------------------------------------: |
| `SIGLA_LG_FACILITY`      | `SHP_FACILITY_ID`    | Sigla da unidade logística (Service Center)     |
| `REGIONAL_GERENTE_OTR`   | `OTR_REGIONAL`       | Nome da regional associada à unidade logística  |

---

🔁 **Joins e Multiplicadores:**

Joins: *Não há operações de join neste step. Os dados são extraídos diretamente da tabela de referência nominal.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- A correspondência entre `SIGLA_LG_FACILITY` e `OTR_REGIONAL` segue a estrutura de regionalização vigente, definida manualmente na base nominal `GSHEET_NOMINAL_MLB`.
- Essa segmentação reflete critérios internos da operação e pode sofrer alterações conforme reorganizações logísticas.

---

🔍 **Considerações técnicas:**

- A tabela `STG.LK_BASE_REGIONAIS` é criada utilizando `CREATE OR REPLACE TABLE`, sobrescrevendo conteúdos anteriores.
- A seleção é direta, sem filtros ou transformações adicionais.
- Essa base serve de apoio para joins com rotas e shipments na construção dos demais steps.

---

⚠️ **Pontos de atenção:**

Essa base depende da atualização manual da planilha [`GSHEET_NOMINAL_MLB`](https://docs.google.com/spreadsheets/d/12XrNqVb1Kpm3lTDyU-J2Iv-v1zSOJgmc5ineQ3oPPuE).

Caso a sigla de alguma unidade não esteja presente nesta base, ela ficará sem regional atribuída nos steps seguintes.
