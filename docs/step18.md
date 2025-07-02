### Step 18 – DE-PARA Veículo: Adicional Parada

🔹 Step 18 – `LK_DEPARA_VEICULO_RECEBE_ADICIONAL_PARADA_CUSTOS`

✅ **Objetivo:**

Criar uma base de mapeamento dos modais utilizados na operação logística, indicando se recebem adicional de parada e/ou custo complementar. Essa base serve de referência para as regras finais de cálculo de custo por rota.

---

⚙️ **Fonte de dados:**

**Tabela utilizada:**
- `meli-bi-data.SBOX_CASECENTERMLB.DE_PARA_VEICULO_TABLEAU`

**Filtro aplicado:**
- *Nenhum filtro foi aplicado neste step.*

---

📐 **Transformações e Seleções:**

| **Coluna no Input** | **Coluna no Output** | **Descrição**                                                    |
|:-------------------:|:-------------------:|:----------------------------------------------------------------:|
| `PARA_2025`         | `MODAL`             | Nome do modal logístico utilizado                                |
| `RECEBE_COMPLEMENTAR` | `RECEBE_COMPLEMENTAR` | Flag se o modal recebe custo complementar (`S` ou `N`)        |
| `RECEBE_ADICIONAL_PARADA` | `RECEBE_ADICIONAL_PARADA` | Flag se o modal recebe adicional de parada (`S` ou `N`)    |

---

🔁 **Joins e Multiplicadores:**

Joins:  
*Esta base será utilizada em joins posteriores, principalmente para aplicação de regras de custo e adicionais por modal. Não há joins diretos neste step.*

Multiplicadores:  
*Nenhum multiplicador aplicado neste step.*

---

📋 **Regras de Negócio Implícitas:**

- O campo `RECEBE_ADICIONAL_PARADA = 'S'` indica que o modal recebe custo adicional em determinadas paradas.
- O campo `RECEBE_COMPLEMENTAR = 'S'` pode ser usado para outras lógicas, como rateio ou custos complementares em etapas posteriores.

---

🔍 **Considerações técnicas:**

- A tabela `STG.LK_DEPARA_VEICULO_RECEBE_ADICIONAL_PARADA_CUSTOS` é criada com `CREATE OR REPLACE TABLE`, sobrescrevendo a anterior.
- Granularidade por `MODAL`, sem distinção por canal ou produto.
- Dados importados diretamente da base DE-PARA mantida pela operação.

---

⚠️ **Pontos de atenção:**

- A padronização dos nomes de modal (`PARA_2025`) é fundamental para garantir que os joins nas etapas seguintes funcionem corretamente.
- Flags incorretas podem gerar distorções no custo final calculado nas rotas.
- Qualquer alteração nessa base pode afetar diversas etapas do cálculo de custos logísticos.

---

