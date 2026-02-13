---
name: debug-investigation
description: Skill de investigacao e debug. Usar quando precisa diagnosticar bugs, correlacionar eventos ou fazer analise pos-mortem. Lembrar que mobile nao tem console - usar tela de debug.
allowed-tools: Read, Grep, Glob, Bash
---

# Debug & Investigacao

## REGRA PRINCIPAL: Mobile nao tem console

iOS e Android NAO tem console de debug acessivel. Sempre usar `debugLog()` e verificar na tela `app/debug-iphone.tsx`. O usuario pode compartilhar os logs pelo botao "Compartilhar".

## Fluxo de Investigacao

1. Reproduzir o problema (ou entender o relato)
2. Adicionar `debugLog()` nos pontos suspeitos com categoria correta
3. Pedir ao usuario para ativar debug (5 toques no footer de Settings)
4. Analisar logs na tela de debug ou via compartilhamento
5. Correlacionar eventos por IDs (userId, blockId, sessionId)
6. Confirmar causa raiz
7. Documentar achado

## Checklist de Debug por Sintoma

| Sintoma | Categoria | Verificar |
|---------|-----------|-----------|
| Tela branca | [ui] | _layout.tsx, imports quebrados, erros de render |
| Dados nao salvam | [storage] [queries] | SQLite open, Platform.OS, WebMockDatabase |
| Sync nao funciona | [sync] [network] | network-monitor, sync-queue, tokens |
| Login falha | [auth] | Cognito config, redirect URIs, PKCE |
| Feed vazio | [feed] [queries] | generator.ts, cooldown, filtros |
| App lento | [queue] [queries] | background-save-queue, queries pesadas |

## Como Adicionar Debug Temporario

```typescript
import { debugLog } from '../sync/debug-log';

// Antes da operacao suspeita
debugLog('[queries] iniciando listBlocks cert=' + cert);

// Depois
debugLog('[queries] listBlocks retornou', result.length, 'blocos');

// Em caso de erro
debugLog('[queries] listBlocks falhou erro=' + err.message);
```

## Analise Pos-Mortem

```markdown
## Incidente: [descricao curta]
- Data: YYYY-MM-DD
- Impacto: [o que quebrou]
- Logs relevantes: [copiar da tela de debug]
- Causa raiz: [por que quebrou]
- Correcao: [o que foi feito]
- Prevencao: [o que fazer pra nao repetir]
```

## Quando Encaminhar

- Causa raiz identificada e precisa de fix -> agente dev
- Problema e de infra AWS -> agente aws-specialist
