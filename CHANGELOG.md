# Changelog

## 0.2.0-pilot

Primeira versão distribuída a clientes.

- Motor local (`scripts/solvis.py`, Python 3.9+, só biblioteca padrão) sobre a API pública V1 da Solvis, somente leitura.
- Métricas: volume de avaliações e respostas, NPS, média de escala, soma ponderada, distribuição de alternativas, múltipla resposta, menções de tags e resumo de Data Quality.
- Recortes por unidade, propriedade de unidade, UF, tempo, dia da semana, faixa de horário, canal, completude, confiabilidade e grupo NPS.
- Comparação entre períodos com teste de significância, verificação de efeito de composição e leitura do fluxo condicional do questionário.
- `validate`: confere as afirmações de um relatório ou apresentação contra o cache, em recálculo independente.
- Atalhos `/comandos`, configuração guiada de credencial (`setup`) e mês inicial de análise.
- Privacidade na ingestão: dados pessoais mascarados e texto LGPD só por opção do usuário.
