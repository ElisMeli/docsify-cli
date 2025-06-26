### Step 15 – Base Rateio M1

🔹 Step 15 – `LK_BASE_RATEIO_M1_CUSTOS`

✅ **Objetivo:**

Construir uma base com o percentual de rateio da jornada entre o trecho Last Mile (`lm_orh_new`) e o trecho First Mile (`fm_orh_new`) dentro do modelo MeliOne. Esse percentual é usado para distribuir corretamente os custos compartilhados entre os trechos da rota.

---

⚙️ **Fonte de dados:**

**Tabela utilizada:**
- `meli-bi-data.SBOX_MELIONEMLB.MeliOne_new`

**Filtro aplicado:**
- `INICIO_ENTREGAS` entre `'2025-01-01'` e `'2025-12-31'`

---

📐 **Transformações e Seleções:**

| *Coluna no output* | *Descrição*                                                                 |
| :------------------| :-------------------------------------------------------------------------- |
| `lm_route_id`       | Identificador da rota logística Last Mile                                  |
| `fm_orh_new`        | Duração da jornada no trecho First Mile (ORH)                              |
| `lm_orh_new`        | Duração da jornada no trecho Last Mile (ORH)                               |
| `PERC_LM`           | Percentual do trecho LM sobre a jornada total (`lm_orh_new / total`)       |

---

🔁 **Joins e Multiplicadores:**

Joins: *Não há joins neste step. A base já contém os valores necessários para o cálculo direto.*

Multiplicadores: *Não há multiplicadores aplicados. A divisão é usada apenas para cálculo proporcional.*

---

📋 **Regras de Negócio Implícitas:**

- Quando `lm_orh_new` é nulo, o percentual é considerado 0.
- Quando `fm_orh_new` é nulo, o percentual é 100% LM.
- A função `SAFE_DIVIDE()` evita erro em divisões por zero.

---

🔍 **Considerações técnicas:**

- A tabela `STG.LK_BASE_RATEIO_M1_CUSTOS` é criada com `CREATE OR REPLACE TABLE`, sobrescrevendo a anterior.
- Os campos `lm_orh_new` e `fm_orh_new` representam as horas trabalhadas em cada trecho.
- A granularidade é por `lm_route_id`.

---

⚠️ **Pontos de atenção:**

- O percentual `PERC_LM` pode distorcer a alocação de custos se os dados de `fm_orh_new` ou `lm_orh_new` estiverem ausentes ou incorretos.
- Certifique-se de que os campos de horas estejam preenchidos e consistentes para todas as rotas válidas.
