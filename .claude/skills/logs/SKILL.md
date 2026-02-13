---
name: logs
description: Skill de logging padronizado para mobile. Usar quando precisa criar, melhorar ou padronizar logs. Logs devem ser filtráveis por categoria, sem emojis, e visíveis na tela de debug do app.
allowed-tools: Read, Grep, Edit
---

# Logging

## REGRA PRINCIPAL: Debug em Mobile

iOS e Android NAO tem console acessivel. Todo log de debug DEVE usar `debugLog()` de `src/sync/debug-log.ts` para aparecer na tela de debug do app (`app/debug-iphone.tsx`).

### Sistema existente no projeto

```typescript
// src/sync/debug-log.ts
import { debugLog } from '../sync/debug-log';

// Loga no console E no buffer in-memory (aparece na tela de debug)
debugLog('[sync] push sucesso blockId=blk-001');

// NUNCA usar console.log para debug temporario
// console.log so para logging permanente/sempre ativo
```

### Tela de debug (app/debug-iphone.tsx)

- Mostra logs em tempo real via subscribe()
- Auto-scroll para o mais recente
- Botao "Compartilhar" -> exporta via Share.share()
- Botao "Limpar" -> reseta buffer
- Ativada via 5 toques no footer de Settings

## Formato (sem emojis, filtravel)

```
[categoria] acao contexto_chave=valor
```

Nunca usar emojis. Sempre usar categoria entre colchetes. Sempre incluir chave=valor para contexto.

## Categorias

| Categoria | Uso |
|-----------|-----|
| [auth] | Login, logout, tokens, sessao |
| [sync] | Fila, push, pull, conflitos |
| [storage] | SQLite, leitura, escrita |
| [feed] | Algoritmo, blocos, prioridades |
| [queries] | Consultas ao banco, resultados |
| [network] | Conectividade, requests |
| [ui] | Navegacao, renderizacao |
| [quiz] | Respostas, score |
| [queue] | Background save queue |
| [cache] | Cache hit/miss |

## Exemplos por Categoria

### [queries]

```typescript
debugLog('[queries] listBlocks retornou', blocks.length, 'blocos cert=' + cert, 'lang=' + lang);
debugLog('[queries] getProgress userId=' + userId, 'blockId=' + blockId, 'score=' + score);
debugLog('[queries] insertProgress sucesso blockId=' + blockId, 'duracao=' + ms + 'ms');
debugLog('[queries] updateScore falhou userId=' + userId, 'erro=' + err.message);
```

### [sync]

```typescript
debugLog('[sync] enqueue', type, 'blockId=' + blockId, 'queueSize=' + size);
debugLog('[sync] push sucesso blockId=' + blockId, 'tentativa=' + retry);
debugLog('[sync] push falhou blockId=' + blockId, 'tentativa=' + retry, 'erro=' + err.message);
debugLog('[sync] pull completo', items.length, 'itens atualizados');
debugLog('[sync] conflito resolvido blockId=' + blockId, 'local=' + localScore, 'cloud=' + cloudScore, 'resultado=' + Math.max(localScore, cloudScore));
```

### [auth]

```typescript
debugLog('[auth] login iniciado provider=' + provider);
debugLog('[auth] token renovado expiresIn=' + expiresIn);
debugLog('[auth] logout userId=' + userId);
debugLog('[auth] sessao expirada, redirecionando para login');
```

### [feed]

```typescript
debugLog('[feed] gerou feed', total, 'blocos (' + novos + ' novos, ' + revisao + ' revisao)', 'cert=' + cert);
debugLog('[feed] bloco em cooldown blockId=' + blockId, 'voltaEm=' + cooldownMin + 'min');
```

## Niveis

| Nivel | Quando | Funcao |
|-------|--------|--------|
| DEBUG | Detalhes internos, so em dev | `debugLog()` |
| INFO | Eventos normais sempre ativos | `console.log()` |
| WARN | Inesperado mas recuperavel | `console.warn()` |
| ERROR | Falha que impede operacao | `console.error()` |

## Regras

- NUNCA emojis
- NUNCA logar tokens completos, senhas ou PII
- Tokens: logar apenas ultimos 4 caracteres
- Sempre incluir IDs de correlacao (userId, blockId, sessionId)
- `debugLog()` para debug (aparece na tela) / `console.log` para permanente
- Formato filtravel: usuario pode buscar por `[queries]` ou `[sync]` na tela de debug
