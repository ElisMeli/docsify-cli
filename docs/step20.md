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

| *Indicador*              | *Descrição*                                                                 |
| :------------------------| :-------------------------------------------------------------------------- |
| `QTDE_PACOTES`           | Total de pacotes únicos por rota                                            |
| `QTDE_DESPACHADOS`       | Pacotes com status `on_route`, exceto bloqueados, picked_up, etc.          |
| `REAL_SPR`               | Pacotes reais da rota (exclui at_station, descontenerizados e transferidos)|
| `DELIVERED`              | Pacotes entregues com status `delivered`                                   |
| `DELIVERED_PLACE`        | Pacotes entregues com status `delivered_place`                             |
| `REENTREGAS`             | Pacotes com mais de uma tentativa de entrega (`NUMBER_VISITED > 1`)        |
| `DESCONTEINERIZADOS_ONROUTE` | Pacotes retirados de contêiner e em rota                              |
| `CLAIMS`                 | Pacotes com ocorrência registrada como claim (`CLAIMS`)                    |
| `OUTROS_INSUCESSOS_NAO_MAPEADOS` | Status fora dos padrões mapeados                                |
| `PICKED_UP`, `UNVISITED`, `BUYER_MOVED`, etc. | Insucessos categorizados por sub-status específicos   |

---

🔁 **Joins e Multiplicadores:**

Joins:
- Com `SHP` para extrair número de tentativas.
- Com `CLA` para contar claims registrados.
- Com `DE` para identificar pacotes descontenerizados.

Multiplicadores: *Nenhum aplicado.*

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
