### Step 20 – Status Shipment Geral

🔹 Step 20 – `LK_STATUS_SHIPMENT_CUSTOS`

✅ **Objetivo:**

Gerar uma base consolidada com métricas completas de status e sub-status dos pacotes por rota, abrangendo entregas, insucessos, reentregas, claims, pacotes descontenerizados, entre outros. Essa base é referência para análise de desempenho e qualidade operacional.

---

⚙️ **Fonte de dados:**

**Tabelas utilizadas:**
- `BT_SHP_LG_SHIPMENTS_ROUTES` (`LOGS`)
- `BT_SHP_LG_SHIPMENTS` (`SHP`)
- `DM_SHP_LOGISTICS_CLAIMS` (`CLA`)
- `BT_PACOTES_DESCONTEINERIZADOS_ONROUTE_LM` (`DE`)

**Filtros aplicados:**
- `LOGS.sit_site_id = 'MLB'`
- `LOGS.shp_lg_route_status = 'close'`
- `LOGS.shp_lg_route_init_date BETWEEN '2025-01-01' AND '2025-12-31'`

---

📐 **Transformações e Seleções:**

| **Coluna no Input**           | **Coluna no Output**      | **Descrição**                                                                 |
| :---------------------------: | :----------------------: | :--------------------------------------------------------------------------- |
| LOGS.SHP_LG_ROUTE_ID          | shp_lg_route_id          | Identificador da rota                                                         |
| LOGS.SHP_SHIPMENT_ID          | QTDE_PACOTES             | Total de pacotes únicos por rota                                              |
| LOGS.SHP_SHIPMENT_ID + regras | QTDE_DESPACHADOS         | Pacotes em rota (com ajustes de subtração de exceções)                        |
| LOGS.SHP_SHIPMENT_ID + regras | REAL_SPR                 | Pacotes viáveis para entrega real                                             |
| LOGS.SHP_SHIPMENT_ID + status | DELIVERED                | Entregues com status `delivered`                                              |
| LOGS.SHP_SHIPMENT_ID + status | DELIVERED_PLACE          | Entregues com status `delivered_place`                                        |
| LOGS.SHP_SHIPMENT_ID + regras | REENTREGAS               | Pacotes com mais de uma tentativa de entrega                                  |
| DE.SHP_SHIPMENT_ID            | DESCONTEINERIZADOS_ONROUTE| Pacotes retirados de contêiner e em rota                                      |
| CLA.SHP_SHIPMENT_ID           | CLAIMS                   | Pacotes com ocorrência registrada como claim                                  |
| LOGS.SHP_SHIPMENT_ID + regras | OUTROS_INSUCESSOS_NAO_MAPEADOS | Status fora dos padrões conhecidos                                   |
| LOGS.SHP_SHIPMENT_ID + substatus | PICKED_UP, UNVISITED, BUYER_MOVED, etc. | Indicadores por insucesso específico    |

**Campos por sub-status de insucesso:**
- `PICKED_UP`: picked_up
- `UNVISITED`: unvisited_address
- `BUYER_MOVED`: buyer_moved
- `DAMAGED`: damaged
- `BUYER_REJECTED`: buyer_rejected
- `BAD_ADDRESS`: bad_address
- `BUYER_ABSENT`: buyer_absent
- `BUSINESS_CLOSED`: business_closed
- `MISSROUTED`: missrouted
- `INACCESSIBLE_ADDRESS`: inaccessible_address
- `MISSING`: missing
- `PROBLEM_SOLVING`: problem_solving
- `RETURN`: return
- `STOLEN`: stolen
- `BLOCKED_BY_KEYWORD`: blocked_by_keyword
- `OTHERS`: attempted_robbery, awaiting_police_report
- `TRANSFERRED`: transferred

---

🔁 **Joins e Multiplicadores:**

Joins:
- Com `SHP` para extrair número de tentativas e sub-status.
- Com `CLA` para contar claims registrados.
- Com `DE` para identificar pacotes descontenerizados.

Multiplicadores: *Nenhum aplicado neste step.*

---

📋 **Regras de Negócio Implícitas:**

- A lógica de `QTDE_DESPACHADOS` subtrai condições específicas do universo `on_route`.
- `REAL_SPR` é a métrica preferencial de contagem de pacotes realmente entregáveis na rota.
- O campo `OUTROS_INSUCESSOS_NAO_MAPEADOS` captura sub-status ainda não formalizados na taxonomia.

---

🔍 **Considerações técnicas:**

- A tabela `STG.LK_STATUS_SHIPMENT_CUSTOS` é sobrescrita com `CREATE OR REPLACE TABLE`.
- A granularidade é por `shp_lg_route_id`.
- As métricas são extraídas com `COUNT DISTINCT` aplicadas a condições específicas.

---

⚠️ **Pontos de atenção:**

- Duplicidades de `SHIPMENT_ID` ou múltiplas ocorrências com o mesmo status podem interferir nos contadores.
- A definição e inclusão de novos sub-status devem ser constantemente revisadas.
- O indicador `REAL_SPR` é o mais próximo da realidade operacional de pacotes viáveis para entrega.
