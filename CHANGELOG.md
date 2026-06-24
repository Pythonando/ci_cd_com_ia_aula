# Changelog

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
