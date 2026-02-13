---
name: general-languages
description: Skill para linguagens alem de JS/Python (Go, Rust, Shell, SQL, etc). Usar quando a tarefa envolve linguagens secundarias ou scripts de automacao.
allowed-tools: Read, Grep, Bash, Edit, Write
---

# Linguagens Gerais

## Referencia Rapida por Linguagem

| Linguagem | Docs |
|-----------|------|
| Go | https://go.dev/doc/ |
| Rust | https://doc.rust-lang.org/book/ |
| Bash/Shell | https://www.gnu.org/software/bash/manual/ |
| SQL | https://www.postgresql.org/docs/current/sql.html |
| YAML | https://yaml.org/spec/1.2.2/ |
| JSON Schema | https://json-schema.org/learn/ |

## Padroes Gerais

- Sempre usar a versao mais recente estavel da linguagem
- Seguir o style guide oficial de cada linguagem
- Preferir stdlib antes de dependencias externas
- Tratar erros explicitamente (nunca silenciar)

## Exemplos de Referencia

### Bash - Script seguro

```bash
#!/usr/bin/env bash
set -euo pipefail

readonly SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
readonly LOG_FILE="${SCRIPT_DIR}/output.log"

log() { echo "[$(date '+%Y-%m-%d %H:%M:%S')] $*" | tee -a "$LOG_FILE"; }

main() {
  local env="${1:?Uso: $0 <environment>}"
  log "Iniciando deploy para ${env}"
  # ...
}

main "$@"
```

### SQL - Query com CTE

```sql
WITH active_users AS (
  SELECT user_id, COUNT(*) as session_count
  FROM sessions
  WHERE created_at > NOW() - INTERVAL '30 days'
  GROUP BY user_id
)
SELECT u.name, au.session_count
FROM users u
JOIN active_users au ON u.id = au.user_id
ORDER BY au.session_count DESC;
```

## Quando Buscar Docs Atualizadas

- Qualquer linguagem que nao seja JS/TS ou Python
- Sempre buscar a doc oficial da versao em uso
