---
name: git
description: Skill de Git e GitHub. Usar quando precisa de comandos git, workflows de branching, PRs, ou resolucao de conflitos.
allowed-tools: Read, Grep, Bash
---

# Git & GitHub

## Documentacao Oficial

- Git Reference: https://git-scm.com/docs
- GitHub CLI: https://cli.github.com/manual/
- Conventional Commits: https://www.conventionalcommits.org/en/v1.0.0/
- Pro Git Book: https://git-scm.com/book/en/v2

## Comandos Essenciais

### Status e Diff

```bash
# Ver estado atual
git status

# Resumo do que mudou
git diff --stat

# Diff detalhado de um arquivo
git diff src/feed/generator.ts

# Diff do que ja esta staged
git diff --cached
```

### Staging e Commit

```bash
# Adicionar arquivos especificos
git add src/feed/generator.ts src/feed/types.ts

# Adicionar por padrao
git add 'src/feed/*.ts'

# Commit com mensagem
git commit -m "feat(feed): adicionar balanceamento por topico"

# Commit com corpo
git commit -m "fix(sync): corrigir retry infinito

Quando o token expirava durante o retry, o loop nao parava.
Agora verifica token antes de cada tentativa."

# Amend no ultimo commit (sem mudar mensagem)
git commit --amend --no-edit
```

### Branches

```bash
# Criar e trocar
git checkout -b feature/nova-feature

# Listar branches
git branch -a

# Deletar branch local
git branch -d feature/antiga

# Deletar branch remota
git push origin --delete feature/antiga
```

### Merge e Rebase

```bash
# Merge com commit
git merge feature/nova-feature --no-ff

# Rebase interativo (ultimos 3 commits)
git rebase -i HEAD~3

# Rebase na main
git rebase main

# Abortar rebase
git rebase --abort
```

### Stash

```bash
# Guardar mudancas
git stash push -m "wip: ajuste no feed"

# Listar stashes
git stash list

# Aplicar e remover
git stash pop

# Aplicar sem remover
git stash apply stash@{0}
```

### Log e Historico

```bash
# Log compacto
git log --oneline -20

# Log com grafo
git log --oneline --graph --all -20

# Log de um arquivo
git log --oneline -- src/feed/generator.ts

# Quem mudou cada linha
git blame src/feed/generator.ts
```

### GitHub CLI (gh)

```bash
# Criar PR
gh pr create --title "feat(feed): balanceamento por topico" --body "Descricao"

# Listar PRs abertas
gh pr list

# Ver PR especifica
gh pr view 42

# Fazer checkout de uma PR
gh pr checkout 42

# Criar issue
gh issue create --title "Bug: sync loop" --body "Descricao"

# Listar issues
gh issue list
```

### Desfazer

```bash
# Desfazer ultimo commit (manter mudancas)
git reset --soft HEAD~1

# Desfazer ultimo commit (descartar mudancas)
git reset --hard HEAD~1

# Reverter um commit especifico (cria novo commit)
git revert abc1234

# Descartar mudancas em arquivo
git checkout -- src/feed/generator.ts
```

## Workflow de Branch

```
main (producao)
  └── develop (integracao)
        ├── feature/nome-da-feature
        ├── fix/descricao-do-bug
        └── chore/descricao-da-tarefa
```

## Regras de Mensagem de Commit

- SEMPRE em Portugues (pt-BR)
- NUNCA usar emojis
- Maximo 2 linhas (titulo + 1 linha de contexto), a menos que o usuario peca mais
- Objetiva e clara, sem enrolacao
- Formato: `tipo(escopo): descricao curta em portugues`

### Exemplos corretos

```bash
git commit -m "feat(feed): adicionar balanceamento por topico no algoritmo"

git commit -m "fix(sync): corrigir retry infinito quando token expira
verifica validade do token antes de cada tentativa"

git commit -m "refactor(storage): extrair queries para arquivo separado"

git commit -m "chore(config): atualizar regras do eslint para flat config"
```

### Exemplos ERRADOS (nao fazer)

```bash
# ERRADO: ingles
git commit -m "feat(feed): add topic balancing"

# ERRADO: emoji
git commit -m "feat(feed): ✨ adicionar balanceamento"

# ERRADO: vago
git commit -m "fix: corrigir bug"

# ERRADO: longo demais
git commit -m "fix(sync): corrigir o problema que acontecia quando o token expirava durante o processo de retry da fila de sincronizacao que causava um loop infinito no processamento"
```

## Regras de Seguranca

- NUNCA `git push --force` sem confirmar
- NUNCA commitar: `.env`, tokens, credentials, `amplify_outputs.json` (ja no .gitignore)
- Sempre verificar `git diff --cached` antes de commitar
- Sempre verificar `.gitignore` antes de adicionar arquivos novos
