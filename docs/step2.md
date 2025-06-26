### Step 02 - Base Loyalty Flat

🔹 Step 02 – `LK_BASE_LOYALTY_FLAT`

✅ **Objetivo:**

Fornecer uma base de apoio para identificar os agrupamentos de motoristas que fazem parte do modelo de bonificação Loyalty, com granularidade por período, site e nível.

---

⚙️ **Fonte de dados:**

**📄 Tabela:** `meli-bi-data.SBOX_CASECENTERMLB.LK_BASE_LOYALTY_FLAT`

**🔎 Filtro aplicado:** *Nenhum filtro aplicado neste step.*

---

📐 **Transformações e Seleções:**

| **Coluna no output** | **Descrição**                                                                 |
| :------------------- | :---------------------------------------------------------------------------- |
| `USER_ID`            | Identificador do entregador                                                   |
| `DRIVER_ID`          | Identificador técnico do driver vinculado ao USER_ID                         |
| `SITE_ID`            | Site de operação do entregador (ex: MLB)                                      |
| `PERIOD_ID`          | Período de vigência do agrupamento Loyalty                                    |
| `LEVEL`              | Nível do entregador no programa de bonificação (ex: Bronze, Prata, Ouro)      |
| `MODE`               | Modal logístico em que o entregador atua (ex: Fixa, Tercerizado, Independente) |

---

🔁 **Joins e Multiplicadores:**

Joins: *Não há operações de join neste step. Os dados já estão consolidados por entregador e período.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

A coluna `LEVEL` define a faixa de bonificação do entregador no período.

`MODE` é usada para segmentar a aplicação de regras diferentes por tipo de vínculo logístico.

`PERIOD_ID` é o delimitador temporal para o qual as regras de Loyalty são válidas.

---

🔍 **Considerações técnicas:**

Esta base é consultada diretamente como `SELECT *`, sem transformações adicionais.

Ela serve de lookup para cruzamento com bases de execução e cálculo de incentivos.

---

⚠️ **Pontos de atenção:** A consistência da base depende da atualização correta dos níveis e modos dos entregadores. `PERIOD_ID` deve estar em conformidade com os ciclos logísticos definidos pela operação.
