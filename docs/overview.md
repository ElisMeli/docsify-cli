# Visão Geral

---

**Tabela de Consulta:**  
`meli-bi-data.WHOWNER.BT_BASEROTASLM_CUSTOS`

**Nome do Projeto:**  
Performance Custos LM

**Objetivo:**  
Consolidar e disponibilizar informações operacionais, logísticas, financeiras e de performance das rotas Last Mile (incluindo MeliOne), em uma base única e pronta para análise e dashboards de custos.

**Frequência de Atualização:**  
Diária (via agendamento no Dataflow)

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

---git add docs/overview.md
---git commit -m "Inclui nova página Visão Geral (overview.md)"
---git push origin master
