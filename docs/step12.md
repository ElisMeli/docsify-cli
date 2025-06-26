### Step 12 – Buffer Status

🔹 Step 12 – `LK_SHIPMENT_BUFFER_STATUS_CUSTOS`

✅ **Objetivo:**

Identificar se um pacote (`SHIPMENT_ID`) possui opção de entrega do tipo `buffered`, sinalizando se está elegível para tratamento de contingência ou janelas de tempo ampliadas. Essa base atribui um `flag` por shipment.

---

⚙️ **Fonte de dados:**

**Tabela utilizada:**
- `meli-bi-data.SHIPPING_BI.BT_SHP_MT_SHIPMENT_SNAPSHOT`

**Filtros aplicados:**
- `SITE_ID = 'MLB'`
- `SNAPSHOT_DATE_CREATED BETWEEN '2025-01-01' AND '2025-12-31'`

---

📐 **Transformações e Seleções:**

| *Coluna no output* | *Descrição*                                                                    |
| :------------------| :------------------------------------------------------------------------------ |
| `SHIPMENT_ID`       | Identificador único do pacote                                                   |
| `buff_flag`         | Flag booleano indicando se há ao menos uma `DELIVERY_OPTION` do tipo `buffered` |

---

🔁 **Joins e Multiplicadores:**

Joins: *Não há joins externos neste step. A lógica se baseia na estrutura interna da coluna aninhada `DELIVERY_OPTIONS`.*

Multiplicadores: *Não há aplicação de multiplicadores neste step.*

---

📋 **Regras de Negócio Implícitas:**

- A condição `TYPE = 'buffered'` dentro do array `DELIVERY_OPTIONS` indica elegibilidade ao modelo de entrega com buffer.
- A flag `buff_flag` é atribuída como `TRUE` se **ao menos uma** opção `buffered` estiver presente para o `SHIPMENT_ID`.

---

🔍 **Considerações técnicas:**

- A tabela utiliza `UNNEST()` sobre o array `DELIVERY_OPTIONS` para analisar os tipos de entrega vinculados ao pacote.
- A criação é feita com `CREATE OR REPLACE TABLE`, sobrescrevendo `STG.LK_SHIPMENT_BUFFER_STATUS_CUSTOS`.
- A lógica de filtro é reforçada com `QUALIFY` para garantir que apenas pacotes com `buffered` de fato entrem no resultado final.

---

⚠️ **Pontos de atenção:**

- Pacotes sem a estrutura `DELIVERY_OPTIONS` devidamente preenchida podem não ser considerados, mesmo que sejam elegíveis.
- A granularidade da base é por `SHIPMENT_ID`, com um único valor booleano representando a condição de buffer.
- A análise considera apenas o snapshot dentro do ano de 2025 — dados fora dessa janela são ignorados.
