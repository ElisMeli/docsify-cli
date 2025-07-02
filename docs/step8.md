### Step 08 - Base Ciclo Rota

🔹 Step 08 – `LK_CICLO_ROTA_CUSTOS`

✅ **Objetivo:**

Criar uma base de apoio que associa cada rota ao seu respectivo ciclo operacional (`CICLO_OPS`), utilizado para análises temporais, agrupamentos por períodos de execução e comparações com budget e planejamento.

---

⚙️ **Fonte de dados:**

**Tabela utilizada:**
- `meli-bi-data.SBOX_CASECENTERMLB.CICLO_ROTA_LM`

**Descrição:** Contém os ciclos operacionais atribuídos por rota, utilizados para segmentação temporal em análises de performance.

**Filtro aplicado:**
- *Nenhum filtro foi aplicado neste step.*

---

📐 **Transformações e Seleções:**

| **Coluna no Input** | **Coluna no Output** | **Descrição**                                                       |
| :-----------------: | :------------------: | :-----------------------------------------------------------------: |
| `SHP_LG_ROUTE_ID`   | `SHP_LG_ROUTE_ID`    | Identificador da rota logística                                     |
| `CICLO_OPS`         | `CICLO_OPS`          | Ciclo operacional associado à rota (semana, período ou janela)      |

---

🔁 **Joins e Multiplicadores:**

Joins: *Não há operações de join neste step. A base já traz as associações entre rotas e ciclos prontos para uso.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- O campo `CICLO_OPS` é atribuído a cada `SHP_LG_ROUTE_ID` conforme a lógica operacional definida no pipeline de upstream.
- Pode representar semanas logísticas, turnos ou outros agrupadores operacionais utilizados em análises.

---

🔍 **Considerações técnicas:**

- Criação realizada com `CREATE OR REPLACE TABLE`, sobrescrevendo a tabela `STG.LK_CICLO_ROTA_CUSTOS`.
- Granularidade por `SHP_LG_ROUTE_ID`, garantindo um ciclo por rota.

---

⚠️ **Pontos de atenção:**

- A atualização correta da base `CICLO_ROTA_LM` é essencial para o alinhamento temporal das análises.
- Ela é dependente da atualização do Ops Clock, que é feita por este arquivo (gerenciada pela equipe Flow):  
[Ops Clock - Google Sheets](https://docs.google.com/spreadsheets/d/16n6QYXPFzHfpXAo6J-77ADY1k9mylIxZv7K5ca-hJfc)

---
