### Step 12 - Buffer Status

🔹 Step 12 – `LK_SHIPMENT_BUFFER_STATUS_CUSTOS`

✅ **Objetivo:**

Identificar se um pacote (`SHIPMENT_ID`) possui opção de entrega do tipo `buffered`, sinalizando se está elegível para tratamento de contingência ou janelas de tempo ampliadas. Essa base atribui um `flag` por shipment.

---

⚙️ **Fonte de dados:**

**Tabela utilizada:**
- `meli-bi-data.SHIPPING_BI.BT_SHP_MT_SHIPMENT_SNAPSHOT`

**Descrição:** Contém snapshots diários dos pacotes, com informações de opções de entrega (`DELIVERY_OPTIONS`), status e dados de roteirização.

**Filtro aplicado:**
- `SITE_ID = 'MLB'`
- `SNAPSHOT_DATE_CREATED BETWEEN '2025-01-01' AND '2025-12-31'`

---

📐 **Transformações e Seleções:**

| **Coluna no Input** | **Coluna no Output** | **Descrição**                                                         |
| :------------------:| :-------------------:| :--------------------------------------------------------------------:|
| `SHIPMENT_ID`       | `SHIPMENT_ID`        | Identificador único do pacote                                         |
| `DELIVERY_OPTIONS`  | `buff_flag`          | Flag booleano indicando se há ao menos uma opção do tipo `buffered`   |

---

🔁 **Joins e Multiplicadores:**

Joins: *Não há joins externos neste step. A análise é feita diretamente via `UNNEST()` na coluna `DELIVERY_OPTIONS`.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- `buff_flag = TRUE` é atribuído quando pelo menos uma `DELIVERY_OPTION` do tipo `'buffered'` está presente para o `SHIPMENT_ID`.
- Pacotes sem opções de entrega listadas não são considerados como `buffered`.

---

🔍 **Considerações técnicas:**

- Utiliza `UNNEST()` para expandir o array `DELIVERY_OPTIONS` e verificar a existência do tipo `'buffered'`.
- Criação com `CREATE OR REPLACE TABLE`, sobrescrevendo a `STG.LK_SHIPMENT_BUFFER_STATUS_CUSTOS`.
- O uso de `QUALIFY` garante que apenas pacotes com `buffered` de fato componham o resultado final.

---

⚠️ **Pontos de atenção:**

- Pacotes sem `DELIVERY_OPTIONS` corretamente preenchidos podem não ser considerados mesmo que sejam elegíveis.
- A granularidade é por `SHIPMENT_ID`, com o `buff_flag` sendo `TRUE` ou `FALSE`.
- A análise considera apenas snapshots de 2025 — dados fora dessa janela são descartados.

---
