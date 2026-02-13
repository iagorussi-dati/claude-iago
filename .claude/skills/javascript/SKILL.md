---
name: javascript
description: Skill de JavaScript e TypeScript. Usar quando a tarefa envolve codigo JS/TS, async/await, modulos, tipos, ou APIs do ecossistema Node/browser.
allowed-tools: Read, Grep, Bash, Edit, Write
---

# JavaScript & TypeScript

## Documentacao Oficial

- TypeScript Handbook: https://www.typescriptlang.org/docs/handbook/
- MDN JavaScript: https://developer.mozilla.org/en-US/docs/Web/JavaScript
- Node.js API: https://nodejs.org/docs/latest/api/

## Padroes do Projeto

- TypeScript strict mode, evitar `any`
- ES Modules (import/export)
- Async/await (nunca .then() encadeado)
- Desestruturacao quando melhora legibilidade

## Exemplos de Referencia

### Tipagem correta de funcoes async

```typescript
// BOM - tipo de retorno explicito
async function fetchUser(id: string): Promise<User | null> {
  try {
    const response = await api.get(`/users/${id}`);
    return response.data;
  } catch {
    return null;
  }
}

// RUIM - any implicito
async function fetchUser(id) {
  const response = await api.get(`/users/${id}`);
  return response.data;
}
```

### Type guards

```typescript
function isApiError(error: unknown): error is { message: string; code: number } {
  return (
    typeof error === 'object' &&
    error !== null &&
    'message' in error &&
    'code' in error
  );
}
```

### Generics uteis

```typescript
type AsyncResult<T> = { data: T; error: null } | { data: null; error: string };

async function safeAsync<T>(fn: () => Promise<T>): Promise<AsyncResult<T>> {
  try {
    return { data: await fn(), error: null };
  } catch (e) {
    return { data: null, error: e instanceof Error ? e.message : String(e) };
  }
}
```

### Map/Record tipado

```typescript
const handlers: Record<string, (payload: unknown) => void> = {
  created: handleCreated,
  updated: handleUpdated,
  deleted: handleDeleted,
};
```

## Quando Buscar Docs Atualizadas

- APIs novas do Node.js (fetch nativo, test runner, etc.)
- Mudancas no TypeScript 5.x+ (satisfies, const type params, etc.)
- Sempre consultar: https://www.typescriptlang.org/docs/handbook/release-notes/overview.html
