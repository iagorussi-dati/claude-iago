---
name: lint
description: Skill de linting e formatacao. Usar apos escrever ou modificar codigo para garantir qualidade e consistencia.
allowed-tools: Read, Grep, Bash, Edit
---

# Lint & Formatacao

## Documentacao Oficial

- ESLint: https://eslint.org/docs/latest/
- ESLint Flat Config: https://eslint.org/docs/latest/use/configure/configuration-files
- typescript-eslint: https://typescript-eslint.io/getting-started/
- Prettier: https://prettier.io/docs/en/

## Config do Projeto

- ESLint com `.eslintrc.js` (verificar se migrou para flat config)
- Prettier: 100 chars, aspas simples, trailing commas es5, 2 espacos, ponto-e-virgula
- Variaveis nao usadas sao erro (prefixo `_` para ignorar)

## Comandos

```bash
# Verificar
npm run lint

# Auto-fix
npm run lint:fix

# Formatar
npm run format
```

## Fluxo Pos-Codigo

1. Salvar arquivo
2. Rodar `npm run lint` no arquivo modificado
3. Se houver erros auto-fixaveis: `npm run lint:fix`
4. Se houver erros manuais: corrigir e repetir
5. Rodar `npm run format` para garantir formatacao

## Exemplos de Erros Comuns

### Variavel nao usada

```typescript
// ERRO: 'data' is defined but never used
const data = fetchData();

// FIX: prefixar com _ se intencional
const _data = fetchData();

// FIX: remover se nao precisa
fetchData();
```

### Import nao usado

```typescript
// ERRO
import { View, Text, Image } from 'react-native';
// so usa View e Text

// FIX
import { View, Text } from 'react-native';
```

### Prettier conflito

```typescript
// ERRO prettier: linha > 100 chars
const resultado = minhaFuncaoComNomeMuitoGrande(parametro1, parametro2, parametro3, parametro4);

// FIX
const resultado = minhaFuncaoComNomeMuitoGrande(
  parametro1,
  parametro2,
  parametro3,
  parametro4,
);
```

## Quando Buscar Docs Atualizadas

- Migracao para ESLint flat config (eslint.config.js)
- Novas regras do typescript-eslint
- Consultar: https://typescript-eslint.io/rules/
