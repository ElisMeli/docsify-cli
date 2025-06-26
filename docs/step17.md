### Step 17 – Base Custo Rota

🔹 Step 17 – `LK_BASE_CUSTO_ROTA_CUSTOS`

✅ **Objetivo:**

Consolidar os custos de rota incorporando o custo bruto, o rateio de jornadas MeliOne, adicionais por parada, e aplicação de multiplicadores conforme o tipo de veículo e produto. Esta base é a principal estrutura de custo final por rota para análises de Performance LM.

---

⚙️ **Fonte de dados:**

**Tabelas utilizadas:**
- `STG.LK_PRE_BASE_CUSTO_ROTA_CUSTOS` (`BCR`)
- `STG.LK_BASE_RATEIO_M1_CUSTOS` (`M1`)
- `STG.LK_BASE_CUSTO_PARADAS_CUSTOS` (`CP`)
- `STG.LK_MODAL_ROTAS_LM_CUSTOS` (`MODAL_INFO`)
- `STG.LK_DEPARA_VEICULO_RECEBE_ADICIONAL_PARADA_CUSTOS` (`DEPARA`)

---

📐 **Transformações e Seleções:**

| *Coluna no output*         | *Descrição*                                                                 |
| :--------------------------| :-------------------------------------------------------------------------- |
| `CUSTO_ROTA_ROUTE`          | ID da rota                                                                 |
| `CUSTO_ROTA_DRIVER`         | Identificador do motorista                                                 |
| `CUSTO_ROTA_DATA_INICIO`    | Data de execução da rota                                                   |
| `CUSTO_ROTA_COST`           | Custo bruto da rota                                                        |
| `RATEIO_PERC_LM`            | Percentual de jornada LM aplicada em casos MeliOne                         |
| `CUSTO_PARADAS_VARIABLE`    | Custo médio adicional por parada                                           |
| `CPR`                       | Custo com rateio e adicional de parada                                     |
| `CPR_FINAL`                 | Custo ajustado com multiplicadores específicos por modal/produto           |

---

🔁 **Joins e Multiplicadores:**

Joins:  
- *Rateio M1:* `BCR.ROUTE = M1.lm_route_id`  
- *Paradas:* por empresa, motorista, unidade e período  
- *Modal:* por `SHP_LG_ROUTE_ID`  
- *Adicional de parada:* usando igualdade de `MODAL` com tratamento de formatação

Multiplicadores:
- `Rental Utilitário` → `* 1.0484`  
- `Rental` → `* 1.0770`  
- `Veículo de Passeio` → `* 1.1281`  
- `Crowd` → `* 1.04`  
- *Demais modais não recebem multiplicador adicional.*

---

📋 **Regras de Negócio Implícitas:**

- Para rotas `melione`, aplica-se o percentual de rateio (`PERC_LM`) ao custo bruto.
- O adicional por parada só é somado quando o mapeamento (`DEPARA.RECEBE_ADICIONAL_PARADA = 'S'`).
- O custo final é então ajustado com multiplicadores específicos por `MODAL` ou `PRODUCT`.

---

🔍 **Considerações técnicas:**

- A tabela é criada com `CREATE OR REPLACE TABLE`, sobrescrevendo `STG.LK_BASE_CUSTO_ROTA_CUSTOS`.
- A granularidade da base é por rota (`ROUTE`), mantendo o detalhamento por motorista, empresa, facility e período.
- Cálculos utilizam `COALESCE` e `CASE` para garantir robustez e evitar nulos.

---

⚠️ **Pontos de atenção:**

- Dados incorretos no DE-PARA de modal podem afetar os multiplicadores aplicados.
- Ausência de match no join de custo de parada pode resultar em subavaliação de CPR.
- O campo `CPR_FINAL` representa o custo completo da rota após todas as regras de negócio aplicadas.
