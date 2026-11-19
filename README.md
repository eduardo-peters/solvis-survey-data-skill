# solvis-survey-data-skill

Skill para analisar resultados de pesquisas da plataforma Solvis com a sua própria LLM. Versão piloto **0.2.0-pilot**.

A LLM escolhe a consulta e explica o resultado. **Quem calcula é um motor local** (`scripts/solvis.py`), que lê a API pública da Solvis e aplica as mesmas fórmulas das telas de resultados. Cada número vem com `n`, período, filtros e um `query_id` que permite conferir a origem.

## O que ela responde

NPS, médias de escala, distribuição de alternativas, volume de avaliações, comentários e tags, por pesquisa, unidade, período, regional, canal, dia da semana e horário. Faz comparações entre períodos, rankings e diagnóstico de variações, com teste de significância e verificação das afirmações antes de entregar relatórios (`validate`).

O que ela **não** tem: CSAT pronto, taxa de resposta, tom/sentimento dos comentários, benchmark de mercado e previsões. Detalhes em [`references/limitations.md`](skills/solvis-survey-analysis-pilot/references/limitations.md).

## Requisitos

- Um assistente que leia [Agent Skills](https://agentskills.io/specification) e execute comandos (por exemplo Claude Code, Codex, Cursor, Gemini CLI, Kiro ou VS Code com Copilot em modo agente).
- Python 3.9 ou mais novo. Nenhum pacote extra.
- Acesso de rede a `https://sistema.solvis.net.br`.
- Uma credencial da API Solvis (somente leitura).

## Instalação

1. Crie a credencial na Solvis: **Arquivo > Cliente > Lista de credenciais da API**. Prefira uma credencial só para esta skill.
2. Copie a pasta `skills/solvis-survey-analysis-pilot/` para a pasta de skills do seu assistente. O nome da pasta deve continuar `solvis-survey-analysis-pilot`. Exemplos: `~/.claude/skills/` (Claude Code, todas as sessões), `.claude/skills/` ou `.agents/skills/` dentro do projeto.
3. Teste no terminal, dentro da pasta da skill:

   ```bash
   python3 scripts/solvis.py scope
   ```

   Sem credencial, o retorno é `missing_credentials`. Isso é esperado antes do primeiro uso.

## Primeiro uso

Peça ao assistente algo como "quais pesquisas tivemos nos últimos 90 dias?". Ele conduz a configuração:

- pede o usuário e a senha da API e roda `setup`, que testa o login e grava um arquivo `.env` só do seu usuário (permissão 600), fora de qualquer projeto: `~/.config/solvis-survey-analysis/.env` (Windows: `%APPDATA%\solvis-survey-analysis\.env`);
- pergunta a partir de que mês analisar os dados. Quanto mais recente, mais rápido o primeiro download.

Se preferir não digitar a senha no chat, rode você mesmo no terminal (a senha é lida da entrada padrão):

```bash
printf '%s' '<senha>' | python3 scripts/solvis.py setup --username '<usuário>' --password -
```

No PowerShell: `'<senha>' | python scripts\solvis.py setup --username '<usuário>' --password -`. Também valem as variáveis `SOLVIS_API_USERNAME` e `SOLVIS_API_PASSWORD`.

Digite `/ajuda` no assistente para ver os atalhos (`/pesquisas`, `/unidades`, `/unidade XXXX`, `/ranking`, `/comentarios`, `/atualizar`, `/status`, `/limpar`).

> Uma senha digitada no chat fica no histórico da conversa. Ao fim do piloto, gere uma nova credencial ou revogue a atual.

## Privacidade e dados locais

- A skill só **lê** a API. Não grava nada na plataforma.
- O motor guarda um cache local das avaliações, por credencial, em `~/.cache/solvis-survey-analysis/` (Windows: `%LOCALAPPDATA%\solvis-survey-analysis\`).
- No cache não entram IP, identificador de respondente, e-mail, telefone, CPF, CNPJ, datas e valores numéricos digitados. Comentários são guardados com dados pessoais mascarados.
- O texto de perguntas protegidas por LGPD não é guardado, a menos que você autorize com `config --lgpd-text on`.
- Para apagar o cache e o token: `python3 scripts/solvis.py purge --yes`. O `.env` com a credencial permanece.
- O texto dos comentários é tratado como dado, nunca como instrução.

## Suporte e feedback

Se uma resposta parecer errada, copie o `query_id` que aparece na linha de fontes e rode:

```bash
python3 scripts/solvis.py explain --query-id <id>
```

Envie o JSON ao seu contato na Solvis.

## Licença

Uso restrito a clientes da Solvis durante o piloto. Todos os direitos reservados.
