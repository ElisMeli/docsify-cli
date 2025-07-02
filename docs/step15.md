### Step 15 - Base Rateio M1

🔹 Step 15 – `LK_BASE_RATEIO_M1_CUSTOS`

✅ **Objetivo:**

Construir uma base com o percentual de rateio da jornada entre o trecho Last Mile (`lm_orh_new`) e o trecho First Mile (`fm_orh_new`) no modelo MeliOne, utilizado para distribuir corretamente os custos compartilhados entre os trechos da rota.

---

⚙️ **Fonte de dados:**

**Tabela utilizada:**
- `meli-bi-data.SBOX_MELIONEMLB.MeliOne_new`

**Descrição:** Tabela que consolida jornadas do modelo MeliOne, contendo as durações de First Mile e Last Mile para cálculo de rateio de custos.

**Filtro aplicado:**
- `INICIO_ENTREGAS` entre `'2025-01-01'` e `'2025-12-31'`.

---

📐 **Transformações e Seleções:**

| **Coluna no Input** | **Coluna no Output** | **Descrição** |
| :---: | :---: | :--- |
| `lm_route_id` | `lm_route_id` | Identificador da rota logística Last Mile |
| `fm_orh_new` | `fm_orh_new` | Duração da jornada no trecho First Mile (ORH) |
| `lm_orh_new` | `lm_orh_new` | Duração da jornada no trecho Last Mile (ORH) |
| Calculado | `PERC_LM` | Percentual do trecho LM sobre a jornada total (`lm_orh_new / total`) |

---

🔁 **Joins e Multiplicadores:**

Joins: *Não há joins neste step. A base já contém os campos necessários para cálculo direto.*

Multiplicadores: *Não aplicados; a divisão serve apenas para cálculo do percentual.*

---

📋 **Regras de Negócio Implícitas:**

- Se `lm_orh_new` for nulo, o percentual será `0`.
- Se `fm_orh_new` for nulo, o percentual será `100%` (ou seja, `1`).
- O cálculo utiliza `SAFE_DIVIDE` para evitar erros de divisão por zero.

---

🔍 **Considerações técnicas:**

- Criação via `CREATE OR REPLACE TABLE`, sobrescrevendo `STG.LK_BASE_RATEIO_M1_CUSTOS`.
- Granularidade por `lm_route_id`.
- Campos de jornada representam horas trabalhadas em cada trecho (FM e LM).

---

⚠️ **Pontos de atenção:**

- Garantir consistência de preenchimento das horas para rotas válidas antes de análises.

---
