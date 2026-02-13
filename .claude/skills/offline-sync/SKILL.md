---
name: offline-sync
description: Skill de sincronizacao offline-first. Usar quando a tarefa envolve SQLite, sync queue, resolucao de conflitos ou persistencia local.
allowed-tools: Read, Grep, Bash, Edit, Write
---

# Offline Sync

## Arquitetura

```
Acao do Usuario -> progressService.save*() -> SQLite (imediato) -> syncService.enqueue()
                                                                        |
Online? -> processQueue() -> busca cloud -> Math.max(local, cloud) -> push AppSync
                          -> fila vazia -> pullFromCloud() atualiza local
```

## Arquivos Chave

- `src/sync/sync-service.ts` - Orquestra processamento de fila
- `src/sync/sync-queue.ts` - Tabela SQLite de fila
- `src/sync/progress-service.ts` - CRUD de progresso
- `src/sync/network-monitor.ts` - Detecta conectividade
- `src/sync/debug-log.ts` - Buffer de log em memoria
- `src/storage/database.ts` - Schema SQLite

## Regras Criticas

- Dependencia circular: `sync-service` <-> `progress-service` quebrada via `import()` dinamico
- Paginacao DynamoDB: sempre `{ limit: 200 }` com loop `nextToken`
- `background-save-queue`: sempre `await waitForQueueEmpty()` antes de navegar
- Resolucao de conflitos: `Math.max()` para contadores, OR para booleanos, data mais recente para streaks
- Retry: ate 5x com contagem de erros

## Exemplos de Referencia

### Enfileirar operacao

```typescript
await syncQueue.enqueue({
  type: 'UPDATE_PROGRESS',
  payload: { blockId, score, completedAt: new Date().toISOString() },
  retryCount: 0,
});
```

### Resolucao de conflito

```typescript
function mergeProgress(local: Progress, cloud: Progress): Progress {
  return {
    ...local,
    score: Math.max(local.score, cloud.score),
    totalCompleted: Math.max(local.totalCompleted, cloud.totalCompleted),
    isCompleted: local.isCompleted || cloud.isCompleted,
    lastActivity: local.lastActivity > cloud.lastActivity
      ? local.lastActivity
      : cloud.lastActivity,
  };
}
```

### Pull com paginacao

```typescript
async function pullAllFromCloud(tableName: string): Promise<Item[]> {
  const items: Item[] = [];
  let nextToken: string | undefined;

  do {
    const result = await client.models[tableName].list({
      limit: 200,
      nextToken,
    });
    items.push(...result.data);
    nextToken = result.nextToken;
  } while (nextToken);

  return items;
}
```
