# Visão Geral

---

**Tabela de Consumo:**  
`meli-bi-data.WHOWNER.BT_BASEROTASLM_CUSTOS`

**Nome do Projeto:**  
Performance Custos LM

**Objetivo:**  
Consolidar e disponibilizar informações operacionais, logísticas, financeiras e de performance das rotas Last Mile (incluindo MeliOne), em uma base única e pronta para análise e dashboards de custos.

**Frequência de Atualização:**  
Diária (via agendamento no Dataflow)

**Frequência de Execução:**  
O processo roda diariamente em múltiplos horários (UTC), convertidos para o horário de Brasília (GMT-3):

- `01:05` UTC → `22:05` (do dia anterior)
- `04:05` UTC → `01:05`
- `07:05` UTC → `04:05`
- `10:05` UTC → `07:05`
- `13:04` UTC → `10:04`
- `16:05` UTC → `13:05`
- `19:05` UTC → `16:05`
- `21:05` UTC → `18:05`

**Tipo de Atualização:**  
Automática (Data Suite)

**Tempo Médio de Processamento:**  
- Steps 1 a 20: 50 minutos  
- Step 21 (consolidação final): 2 horas  (podendo variar conforme volume de dados)

**Responsável:**  
Elisangela Fernandes  
[elisangela.fernandes@mercadolivre.com](mailto:elisangela.fernandes@mercadolivre.com)

**Supervisora:**  
Thaise Tavares  
[thaise.lima@mercadolivre.com](mailto:thaise.lima@mercadolivre.com)

---

> Para dúvidas, sugestões ou solicitações de ajuste, entre em contato por e-mail ou pelo Chat.


