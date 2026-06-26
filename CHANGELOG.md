# Changelog

## [2026-06-26] - 15
**PR:** #10 por @Pythonando

### O que mudou
O sistema de análise automática de issues foi aprimorado para funcionar de forma mais completa e confiável. Agora ele tem permissão para interagir também com pull requests, exibe o resultado completo das análises realizadas pela IA e consegue executar tarefas mais longas e complexas sem travar no meio do caminho.

### Detalhes técnicos
- **`.github/workflows/claude-code-issues.yml`**: Três ajustes na action de triagem de issues:
  - Adicionada permissão `pull-requests: write` ao job, permitindo que o workflow interaja com PRs quando necessário.
  - Habilitado `show_full_output: 'true'` na action `anthropics/claude-code-action@v1`, garantindo que a saída completa da execução seja exibida nos logs.
  - Adicionado `claude_args` com `--max-turns 30` (aumenta o limite de iterações do agente) e `--dangerously-skip-permissions` (ignora verificações de permissão interativas para execução não-supervisionada em CI).

---

## [2026-06-26] - 13
**PR:** #8 por @Pythonando

### O que mudou
A forma como o sistema de automação se autentica para analisar issues foi atualizada. Antes, era usada uma chave de API direta; agora passa a ser utilizado um token OAuth do Claude Code — um método de autenticação mais seguro e alinhado com as práticas recomendadas pelo serviço.

### Detalhes técnicos
- **`.github/workflows/claude-code-issues.yml`**: Substituição da chave de autenticação `anthropic_api_key` (secret `ANTHROPIC_API_KEY`) pelo campo `claude_code_oauth_token` (secret `CLAUDE_CODE_OAUTH_TOKEN`) na action `anthropics/claude-code-action@v1`. Migração do método de autenticação de API key para OAuth token.

---

## [2026-06-25] - 11
**PR:** #6 por @Pythonando

### O que mudou
O projeto ganhou uma infraestrutura completa de automação com inteligência artificial. Agora o repositório conta com revisão automática de código em cada pull request, triagem automática de issues abertas, geração automática de release notes no CHANGELOG após cada merge, geração automática de testes, e a possibilidade de chamar o Claude diretamente em comentários usando `@claude`. Também foi adicionado o arquivo inicial da aplicação Python e as configurações de permissão do Claude Code.

### Detalhes técnicos
- **`app.py`**: Novo arquivo Python com dois `print` statements ("Hello, World!" e "teste 2"), base inicial da aplicação.
- **`.claude/settings.json`**: Configuração de permissões do Claude Code, habilitando operações `Write`, `Edit` e comandos git (`add`, `commit`, `push`).
- **`.github/workflows/claude-code-issues.yml`**: Novo workflow acionado no evento `issues: opened`. O Claude analisa cada nova issue e posta um comentário estruturado com classificação (tipo, severidade, story points), arquivos afetados, subtarefas sugeridas em checklist, issues relacionadas e observações técnicas.
- **`.github/workflows/claude-code-review.yml`**: Novo workflow acionado em pull requests. Executa revisão de código com foco em segurança (OWASP), queries N+1 e lógica de negócio (via `/code-review` plugin). Inclui step de geração automática de testes pytest para funções sem cobertura e job `release-notes` que atualiza o `CHANGELOG.md` automaticamente após merge.
- **`.github/workflows/claude.yml`**: Novo workflow que permite interagir com o Claude via `@claude` em comentários de issues, PRs e revisões de código.

---

## [2026-06-25] - 11
**PR:** #4 por @Pythonando

### O que mudou
O projeto ganhou uma nova funcionalidade de triagem automática de issues. Agora, toda vez que alguém abre uma issue no repositório, o Claude (IA da Anthropic) analisa o conteúdo automaticamente e publica um comentário estruturado — classificando o tipo de problema, estimando a complexidade, listando os arquivos possivelmente afetados, sugerindo subtarefas e apontando issues relacionadas. Isso agiliza o processo de avaliação e organização do trabalho pela equipe.

### Detalhes técnicos
- **`.github/workflows/claude-code-issues.yml`**: Novo workflow acionado no evento `issues: opened`. Utiliza a action `anthropics/claude-code-action@v1` com um prompt detalhado que instrui o Claude a postar um comentário de triagem contendo:
  - Classificação (tipo, severidade e estimativa de story points)
  - Arquivos ou áreas do código possivelmente afetados
  - Subtarefas sugeridas em formato de checklist GitHub
  - Issues relacionadas abertas no repositório
  - Observações sobre riscos técnicos ou informações faltantes

---

## [2026-06-24] - Teste 3
**PR:** #3 por @Pythonando

### O que mudou
O projeto ganhou automação com inteligência artificial integrada ao fluxo de desenvolvimento. Agora, toda vez que uma alteração é enviada para revisão, o Claude (IA da Anthropic) analisa o código automaticamente, aponta possíveis problemas e sugere melhorias — tudo em português. Além disso, quando um PR é aprovado e mesclado, o sistema gera automaticamente as notas de lançamento no CHANGELOG, sem precisar de intervenção manual. Também é possível pedir ajuda diretamente ao Claude nos comentários de um PR ou issue usando `@claude`.

### Detalhes técnicos
- **`app.py`**: Novo arquivo Python com dois `print` statements ("Hello, World!" e "teste 2"), servindo como base inicial da aplicação.
- **`.claude/settings.json`**: Novo arquivo de configuração de permissões do Claude Code, habilitando operações de escrita, edição e comandos git (`add`, `commit`, `push`).
- **`.github/workflows/claude-code-review.yml`**: Workflow de revisão de código aprimorado com:
  - Disparo no evento `closed` para suportar o job de release notes pós-merge.
  - Revisão restrita a membros, proprietários e colaboradores do repositório.
  - Permissões elevadas para `write` em `contents`, `pull-requests` e `issues`.
  - Comentário fixo (sticky comment) com o resultado da revisão no PR.
  - System prompt customizado em português com foco em segurança (OWASP), queries N+1 e lógica de negócio.
  - Novo step para geração automática de testes pytest em PRs com funções sem cobertura.
  - Novo job `release-notes` executado após merge, responsável por atualizar o `CHANGELOG.md` automaticamente.
- **`.github/workflows/claude.yml`**: Novo workflow que permite interagir com o Claude via `@claude` em comentários de issues, PRs e revisões de código.

---
