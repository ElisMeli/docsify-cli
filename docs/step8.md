### Step 08 – Base Ciclo Rota

🔹 Step 08 – `LK_CICLO_ROTA_CUSTOS`

✅ **Objetivo:**

Criar uma base de apoio que associa cada rota ao seu respectivo ciclo operacional (`CICLO_OPS`), utilizado para análises temporais, agrupamentos por períodos de execução e comparações com budget e planejamento.

---

⚙️ **Fonte de dados:**

**Tabela utilizada:**
- `meli-bi-data.SBOX_CASECENTERMLB.CICLO_ROTA_LM`

**Filtro aplicado:**
- *Nenhum filtro foi aplicado neste step.*

---

📐 **Transformações e Seleções:**

| *Coluna no output*   | *Descrição*                                                        |
| :------------------- | :----------------------------------------------------------------- |
| `SHP_LG_ROUTE_ID`    | Identificador da rota logística                                    |
| `CICLO_OPS`          | Ciclo operacional associado à rota (ex: semana, período ou janela) |

---

🔁 **Joins e Multiplicadores:**

Joins: *Não há operações de join neste step. A base já traz as associações entre rotas e ciclos prontos para uso.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- O campo `CICLO_OPS` é atribuído a cada `SHP_LG_ROUTE_ID` conforme lógica operacional pré-definida no pipeline upstream.
- Pode representar semanas logísticas, turnos ou outros agrupadores operacionais usados para análises recorrentes.

---

🔍 **Considerações técnicas:**

- A criação é feita com `CREATE OR REPLACE TABLE`, sobrescrevendo a tabela `STG.LK_CICLO_ROTA_CUSTOS`.
- A granularidade da base é por rota (`SHP_LG_ROUTE_ID`), com uma coluna adicional de agrupamento (`CICLO_OPS`).

---

⚠️ **Pontos de atenção:**

- É fundamental que os dados de `CICLO_ROTA_LM` estejam atualizados para garantir a correta vinculação temporal das rotas.
- Casos em que `CICLO_OPS` esteja nulo podem comprometer a análise de desempenho por ciclo.
