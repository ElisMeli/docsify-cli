### Step 09 - Base Ciclo Budget

🔹 Step 09 – `LK_CICLO_BUDGET_CUSTOS`

✅ **Objetivo:**

Adaptar a base de ciclos operacionais para alinhar os valores com a nomenclatura orçamentária (Budget), substituindo ciclos específicos quando necessário. Essa base padroniza os ciclos para permitir comparações com planejamentos financeiros e metas.

---

⚙️ **Fonte de dados:**

**Tabela utilizada:**
- `meli-bi-data.SBOX_CASECENTERMLB.CICLO_ROTA_LM`

**Descrição:** Contém os ciclos operacionais atribuídos por rota, sendo referência para alinhamento com o planejamento financeiro.

Esta tabela se conecta ao planejamento armazenado em `meli-bi-data.SBOX_MLB_FORECASTING.MLB_RTG_BUDGET_LM_TARGETS_2025`, onde **para 2025 foram definidos apenas dois ciclos operativos: `AM` e `PM`**. Por isso, a lógica do pipeline converte o ciclo `SD` para `PM`, garantindo consistência com os dados de budget, **sem necessidade de ajuste manual futuro, pois o pipeline já está alinhado com essa estrutura**.

**Filtro aplicado:**
- *Nenhum filtro foi aplicado neste step.*

---

📐 **Transformações e Seleções:**

| **Coluna no Input** | **Coluna no Output** | **Descrição**                                                      |
| :-----------------: | :------------------: | :----------------------------------------------------------------: |
| `shp_lg_route_id`   | `shp_lg_route_id`    | Identificador da rota logística                                    |
| `CICLO_OPS`         | `CICLO_ROTA`         | Ciclo operacional bruto conforme registrado na base original      |
| `CICLO_BUDGET`      | `CICLO_BUDGET`       | Ciclo ajustado para orçamentação (ex: `'SD'` transformado em `'PM'`) |

---

🔁 **Joins e Multiplicadores:**

Joins: *Não há joins neste step. A lógica é aplicada diretamente sobre os dados da tabela de ciclos.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- Quando `CICLO_OPS = 'SD'`, é convertido para `'PM'`, refletindo a nomenclatura usada em budget e metas.
- Nos demais casos, `CICLO_OPS` é mantido como `CICLO_BUDGET`.

---

🔍 **Considerações técnicas:**

- Criação realizada com `CREATE OR REPLACE TABLE`, sobrescrevendo a tabela `STG.LK_CICLO_BUDGET_CUSTOS`.
- Granularidade por `shp_lg_route_id`, mantendo os campos de ciclo bruto e ciclo ajustado.

---

⚠️ **Pontos de atenção:**

- Importante que todos os ciclos utilizados no planejamento financeiro estejam refletidos corretamente na lógica de `CASE`.


---
