### Step 09 – Base Ciclo Budget

🔹 Step 09 – `LK_CICLO_BUDGET_CUSTOS`

✅ **Objetivo:**

Adaptar a base de ciclos operacionais para alinhar os valores com a nomenclatura orçamentária (Budget), substituindo ciclos específicos quando necessário. Essa base padroniza os ciclos para permitir comparações com planejamentos financeiros e metas.

---

⚙️ **Fonte de dados:**

**Tabela utilizada:**
- `meli-bi-data.SBOX_CASECENTERMLB.CICLO_ROTA_LM`

**Filtro aplicado:**
- *Nenhum filtro foi aplicado neste step.*

---

📐 **Transformações e Seleções:**

| *Coluna no output*    | *Descrição*                                                                 |
| :---------------------| :-------------------------------------------------------------------------- |
| `shp_lg_route_id`     | Identificador da rota logística                                             |
| `CICLO_ROTA`          | Ciclo operacional bruto conforme registrado na base original                |
| `CICLO_BUDGET`        | Ciclo ajustado para orçamentação. Ex: `'SD'` transformado em `'PM'`         |

---

🔁 **Joins e Multiplicadores:**

Joins: *Não há joins neste step. A lógica é aplicada diretamente sobre os dados da tabela de ciclos.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- Quando `CICLO_OPS = 'SD'`, é convertido para `'PM'`, refletindo a nomenclatura usada nas metas e budgets.
- Nos demais casos, `CICLO_OPS` é mantido como `CICLO_BUDGET`.

---

🔍 **Considerações técnicas:**

- A tabela `STG.LK_CICLO_BUDGET_CUSTOS` é criada com `CREATE OR REPLACE TABLE`, sobrescrevendo conteúdo anterior.
- A base mantém a granularidade por `shp_lg_route_id`, com duas colunas que representam o ciclo original e o ciclo ajustado.

---

⚠️ **Pontos de atenção:**

- Certifique-se de que todos os ciclos considerados no budget estejam refletidos corretamente na lógica de `CASE`.
- Se novos ciclos forem introduzidos, essa lógica deverá ser revisada para garantir compatibilidade com o planejamento.
