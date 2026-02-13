---
name: debug-investigation
description: Skill de investigacao e debug. Usar quando precisa diagnosticar bugs, correlacionar eventos ou fazer analise pos-mortem. Lembrar que mobile nao tem console - usar tela de debug.
allowed-tools: Read, Grep, Glob, Bash
---

# Debug & Investigacao

## REGRA PRINCIPAL: Mobile nao tem console

iOS e Android NAO tem console de debug acessivel. Sempre usar `debugLog()` e verificar na tela `app/debug-iphone.tsx`. O usuario pode compartilhar os logs pelo botao "Compartilhar".

## Fluxo de Investigacao

### PASSO 0: Perguntas Contextuais (OBRIGATORIO)

ANTES de iniciar qualquer debug, fazer perguntas contextuais baseadas na complexidade do projeto:

#### Analisar o contexto do problema:
1. **Identificar o fluxo envolvido**: Qual parte do sistema esta afetada?
2. **Identificar plataformas**: Web, mobile, ambos?
3. **Identificar recursos**: Banco de dados, API, autenticacao, storage?

#### Fazer perguntas especificas baseadas no erro:

**Exemplo 1: "Feed nao esta mostrando blocos" ou "Feed vazio"**
- Analise: Feed depende de dados locais, algoritmo, cooldown, filtros
- Perguntas contextuais:
  1. "Voce ja completou algum bloco antes ou e a primeira vez usando o app?"
     - [Primeira vez], [Ja usei antes], [Nao sei]
  2. "Esta vendo tela vazia, carregando infinito, ou mensagem de erro?"
     - [Tela vazia], [Carregando], [Erro especifico], [Nao sei]
  3. "Qual plataforma?" → [iOS], [Android], [Web]
- Se "Nao sei": Verificar logs [feed], [queries], [storage] e estado de dados locais

**Exemplo 2: "Progresso nao esta salvando" ou "Dados somem"**
- Analise: Pode ser fila de sync, background queue, conflito, ou problema de persistencia
- Perguntas contextuais:
  1. "O progresso some imediatamente, ao fechar o app, ou ao reabrir?"
     - [Imediatamente], [Ao fechar app], [Ao reabrir], [Nao sei]
  2. "Voce esta online ou offline quando completa o bloco?"
     - [Online], [Offline], [Alternando], [Nao sei]
  3. "Qual plataforma?" → [Mobile], [Web]
- Se "Nao sei": Verificar logs [storage], [sync], [queue] e estado da fila

**Exemplo 3: "App lento" ou "App travando"**
- Analise: Pode ser queries pesadas, background-save-queue sem await, sync loop
- Perguntas contextuais:
  1. "A lentidao e constante ou em momentos especificos?"
     - [Constante], [Ao abrir app], [Ao completar bloco], [Ao navegar], [Nao sei]
  2. "Em qual tela acontece mais?"
     - [Feed], [Quiz], [Settings], [Todas], [Nao sei]
  3. "Qual plataforma?" → [iOS], [Android], [Web]
- Se "Nao sei": Verificar logs [queue], [queries] e performance de operacoes

**Exemplo 4: "Sincronizacao nao funciona" ou "Dados nao sobem pra nuvem"**
- Analise: Token, rede, fila travada, conflito nao resolvido
- Perguntas contextuais:
  1. "Os dados aparecem localmente mas nao na nuvem, ou nao aparecem em lugar nenhum?"
     - [Local sim, nuvem nao], [Nao aparecem], [Aparece duplicado], [Nao sei]
  2. "Voce esta online? Pode verificar o status na tela de debug?"
     - [Online], [Offline], [Nao consigo ver], [Nao sei]
  3. "A fila de sync tem itens pendentes? (verificar na tela debug)"
     - [Sim, tem itens], [Fila vazia], [Nao consigo ver], [Nao sei]
- Se "Nao sei": Verificar logs [sync], [network], estado da fila e tokens

**Exemplo 5: "Login com Google nao funciona"**
- Analise: Redirect URI, PKCE, configuracao por plataforma (iOS/Android/Web tem configs diferentes)
- Perguntas contextuais:
  1. "Qual plataforma?" → [iOS], [Android], [Web]
  2. "O redirect para o Google acontece, mas falha ao voltar pro app?"
     - [Nao redireciona], [Redireciona mas falha na volta], [Erro antes do redirect], [Nao sei]
  3. "Aparece algum erro especifico? (ex: invalid_grant, redirect_uri_mismatch)"
     - [Sim: descrever], [Nao aparece erro], [Nao sei]
- Se "Nao sei": Verificar logs [auth], configuracao Cognito, redirect URIs por plataforma

#### Template de Perguntas:

```
Antes de debugar, preciso entender melhor o contexto:

1. [Pergunta especifica baseada no erro]
   - Opcao A
   - Opcao B
   - Nao sei (vou investigar automaticamente)

2. [Pergunta de contexto se necessario]
   - Opcao A
   - Opcao B
```

### PASSO 1-7: Investigacao Tecnica

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
