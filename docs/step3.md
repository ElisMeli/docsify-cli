### Step 03 – Base Regionais

🔹 Step 03 – `LK_BASE_REGIONAIS`

✅ **Objetivo:**

Criar uma base de apoio para mapear a relação entre unidades logísticas e suas respectivas regionais operacionais, possibilitando segmentações por `REGIONAL` nas análises de Performance de Custos LM.

---

⚙️ **Fonte de dados:**

**Tabela utilizada:**
- `meli-bi-data.SBOX_CASECENTERMLB.GSHEET_NOMINAL_MLB`

**Filtro aplicado:**
- *Nenhum filtro foi aplicado neste step.*

---

📐 **Transformações e Seleções:**

| *Coluna no output*    | *Descrição*                                      |
| :-------------------- | :----------------------------------------------- |
| `SIGLA_LG_FACILITY`   | Sigla da unidade logística (Service Center)      |
| `OTR_REGIONAL`        | Nome da regional associada à unidade logística   |

---

🔁 **Joins e Multiplicadores:**

Joins: *Não há operações de join neste step. Os dados são extraídos diretamente da tabela de referência nominal.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- A correspondência entre `SIGLA_LG_FACILITY` e `OTR_REGIONAL` segue a estrutura de regionalização vigente, definida manualmente na base nominal (`GSHEET_NOMINAL_MLB`).
- Essa segmentação reflete critérios internos da operação e pode sofrer alterações conforme reorganizações logísticas.

---

🔍 **Considerações técnicas:**

- A tabela `STG.LK_BASE_REGIONAIS` é criada com `CREATE OR REPLACE TABLE`, sobrescrevendo qualquer conteúdo anterior.
- A seleção é direta, sem aplicação de filtros ou transformações adicionais.
- Essa base serve de apoio para joins com rotas e shipments na construção dos demais steps.

---

⚠️ **Pontos de atenção:**

- Essa base depende da atualização manual da planilha `GSHEET_NOMINAL_MLB`.
- Mudanças na estrutura regional (como fusões ou renomeações) precisam ser refletidas nesta tabela para manter a consistência das análises.
- Caso a sigla de alguma unidade não esteja presente nessa base, ela pode ficar sem regional atribuída nos steps seguintes.
