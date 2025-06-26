### Step 18 – DE-PARA Veículo: Adicional Parada

🔹 Step 18 – `LK_DEPARA_VEICULO_RECEBE_ADICIONAL_PARADA_CUSTOS`

✅ **Objetivo:**

Criar uma base de mapeamento dos modais utilizados na operação logística com as respectivas flags que indicam se recebem adicional de parada e/ou custo complementar. Essa base é usada como referência em cálculos finais de custo por rota.

---

⚙️ **Fonte de dados:**

**Tabela utilizada:**
- `meli-bi-data.SBOX_CASECENTERMLB.DE_PARA_VEICULO_TABLEAU`

**Filtro aplicado:**
- *Nenhum filtro foi aplicado neste step.*

---

📐 **Transformações e Seleções:**

| *Coluna no output*           | *Descrição*                                                              |
| :----------------------------| :------------------------------------------------------------------------|
| `MODAL`                      | Nome do modal logístico utilizado (`PARA_2025`)                           |
| `RECEBE_COMPLEMENTAR`        | Flag que indica se o modal recebe custo complementar (`S` ou `N`)         |
| `RECEBE_ADICIONAL_PARADA`    | Flag que indica se o modal recebe adicional por parada (`S` ou `N`)       |

---

🔁 **Joins e Multiplicadores:**

Joins: *Essa base é usada em joins posteriores para aplicação de regras de custo. Não há joins diretos neste step.*

Multiplicadores: *Nenhum multiplicador aplicado neste step.*

---

📋 **Regras de Negócio Implícitas:**

- O campo `RECEBE_ADICIONAL_PARADA = 'S'` indica que o modal terá custo adicional em paradas específicas.
- `RECEBE_COMPLEMENTAR = 'S'` pode ser usado em outras lógicas de cálculo complementar, como rateios ou exceções.

---

🔍 **Considerações técnicas:**

- A tabela é criada com `CREATE OR REPLACE TABLE`, sobrescrevendo `STG.LK_DEPARA_VEICULO_RECEBE_ADICIONAL_PARADA_CUSTOS`.
- A granularidade é por `MODAL`, sem repetições por canal ou produto.
- Os dados são importados diretamente de uma base DE-PARA mantida pela operação.

---

⚠️ **Pontos de atenção:**

- A consistência dos nomes dos modais (`PARA_2025`) é fundamental para garantir joins corretos com outras bases.
- Flags incorretas ou incompletas podem impactar diretamente o custo final calculado nas rotas.
