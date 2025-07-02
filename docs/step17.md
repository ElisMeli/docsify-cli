### Step 17 – Base Custo Rota

🔹 Step 17 – `LK_BASE_CUSTO_ROTA_CUSTOS`

✅ **Objetivo:**

Consolidar os custos de rota, incorporando o custo bruto, o rateio de jornadas MeliOne, adicionais por parada e aplicação de multiplicadores conforme o tipo de veículo e produto. Esta base é referência para a análise final dos custos por rota na Performance LM.

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

| **Coluna no Input** | **Coluna no Output** | **Descrição** |
|:-------------------:|:-------------------:|:-------------:|
| `BCR.SHP_COMPANY_ID` | `CUSTO_ROTA_SHP_COMPANY_ID` | ID da empresa contratada |
| `BCR.SHP_COMPANY_NAME` | `CUSTO_ROTA_SHP_COMPANY_NAME` | Nome da empresa |
| `BCR.SHP_LG_PRE_INVOICE_HEADER_PERIOD_NAME` | `CUSTO_ROTA_SHP_LG_PRE_INVOICE_HEADER_PERIOD_NAME` | Período de referência |
| `BCR.ROUTE` | `CUSTO_ROTA_ROUTE` | Identificador da rota |
| `BCR.SHP_FACILITY_ID` | `SHP_FACILITY_ID` | Unidade logística vinculada à rota |
| `BCR.DRIVER` | `CUSTO_ROTA_DRIVER` | Identificador do motorista |
| `BCR.SHP_LG_STEP_TYPE` | `CUSTO_ROTA_SHP_LG_STEP_TYPE` | Etapa da rota (Last Mile/MeliOne) |
| `BCR.DATA_INICIO` | `CUSTO_ROTA_DATA_INICIO` | Data de início da rota |
| `BCR.SHP_LG_PRE_INVOICE_DETAIL_DESCRIPTION` | `CUSTO_ROTA_SHP_LG_PRE_INVOICE_DETAIL_DESCRIPTION` | Descrição do item de cobrança |
| `BCR.SHP_LG_ROUTE_TRAVELED_DISTANCE` | `CUSTO_ROTA_SHP_LG_ROUTE_TRAVELED_DISTANCE` | Distância real percorrida |
| `BCR.SHP_LG_ROUTE_PLANNED_DISTANCE` | `CUSTO_ROTA_SHP_LG_ROUTE_PLANNED_DISTANCE` | Distância planejada da rota |
| `BCR.COST` | `CUSTO_ROTA_COST` | Custo bruto da rota |
| `BCR.SHP_LG_ROUTE_AMOUNT_SHIPMENTS` | `CUSTO_ROTA_SHP_LG_ROUTE_AMOUNT_SHIPMENTS` | Qtd. total de pacotes da rota |
| `BCR.DELIVERED` | `CUSTO_ROTA_DELIVERED` | Qtd. de pacotes entregues com sucesso |
| `BCR.STOPS` | `CUSTO_ROTA_STOPS` | Qtd. de paradas distintas |
| `M1.fm_orh_new` | `RATEIO_FM_ORH_NEW` | Horas de jornada First Mile |
| `M1.lm_orh_new` | `RATEIO_LM_ORH_NEW` | Horas de jornada Last Mile |
| `M1.PERC_LM` | `RATEIO_PERC_LM` | Percentual de rateio LM aplicado |
| `CP.COST` | `CUSTO_PARADAS_COST` | Custo total de paradas |
| `CP.QTY_ROUTES` | `CUSTO_PARADAS_QTY_ROUTES` | Qtd. de rotas associadas |
| `CP.VARIABLE` | `CUSTO_PARADAS_VARIABLE` | Custo médio adicional por parada |
| (Cálculo CPR) | `CPR` | Custo ajustado com rateio e adicional de parada |

---

🔁 **Joins e Multiplicadores:**

**Joins:**  
- Rateio M1: `BCR.ROUTE = M1.lm_route_id`
- Paradas: por empresa, motorista, unidade e período
- Modal: por identificador de rota
- Recebe adicional de parada: por MODAL (normalizado)

**Multiplicadores:**  
*Obs: O cálculo do custo final com multiplicadores será tratado no Step 18. Aqui, o campo CPR já está pronto para esse ajuste posterior.*

---

📋 **Regras de Negócio Implícitas:**

- Para rotas `melione`, aplica-se o percentual de rateio (`PERC_LM`) ao custo bruto.
- O adicional por parada é somado somente se mapeado como elegível (`RECEBE_ADICIONAL_PARADA = 'S'`).
- CPR resulta do custo ajustado por rateio e adicional de parada.

---

🔍 **Considerações técnicas:**

- Tabela criada com `CREATE OR REPLACE TABLE`, sobrescrevendo a anterior.
- Granularidade por rota, com detalhamento por motorista, empresa, unidade e período.
- Uso de funções como `COALESCE` para garantir robustez dos cálculos.
- Este step **não aplica ainda** o multiplicador do modal (isso será feito no Step 18).

---

⚠️ **Pontos de atenção:**

- CPR já é o custo ajustado conforme a regra vigente, mas sem multiplicadores específicos por modal/produto.
- Divergências de dados em entradas (drivers, empresas, unidades) impactam diretamente o valor final.

---

