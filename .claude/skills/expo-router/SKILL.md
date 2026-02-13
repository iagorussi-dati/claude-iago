---
name: expo-router
description: Skill de Expo Router. Usar quando a tarefa envolve navegacao, rotas, layouts, tabs, deep linking ou qualquer coisa de roteamento no app.
allowed-tools: Read, Grep, Bash, Edit, Write
---

# Expo Router

## Documentacao Oficial (SEMPRE consultar para versao atualizada)

- Introducao: https://docs.expo.dev/router/introduction/
- Core Concepts: https://docs.expo.dev/router/basics/core-concepts/
- Notacao: https://docs.expo.dev/router/basics/notation/
- Stack: https://docs.expo.dev/router/advanced/stack/
- Tabs: https://docs.expo.dev/router/advanced/tabs/
- Modals: https://docs.expo.dev/router/advanced/modals/
- Root Layout: https://docs.expo.dev/router/advanced/root-layout/
- API Reference: https://docs.expo.dev/versions/latest/sdk/router/

## Estrutura de Rotas do Projeto

```
app/
├── _layout.tsx          # Root layout (auth guard, DB init)
├── index.tsx            # Redirect inicial
├── login.tsx            # Tela de login
├── callback.tsx         # OAuth callback
├── settings.tsx         # Configuracoes
├── (tabs)/              # Tab navigation
│   └── _layout.tsx      # Tab layout
└── onboarding/          # Fluxo inicial
    └── _layout.tsx
```

## Exemplos de Referencia

### Root layout com auth guard

```typescript
import { Slot, useRouter, useSegments } from 'expo-router';
import { useEffect } from 'react';

export default function RootLayout() {
  const router = useRouter();
  const segments = useSegments();
  const isAuthenticated = useAuthStore((s) => s.isAuthenticated);

  useEffect(() => {
    const inAuthGroup = segments[0] === '(auth)';
    if (!isAuthenticated && !inAuthGroup) {
      router.replace('/login');
    } else if (isAuthenticated && inAuthGroup) {
      router.replace('/(tabs)');
    }
  }, [isAuthenticated, segments]);

  return <Slot />;
}
```

### Tab layout

```typescript
import { Tabs } from 'expo-router';

export default function TabLayout() {
  return (
    <Tabs screenOptions={{ headerShown: false }}>
      <Tabs.Screen name="index" options={{ title: 'Feed' }} />
      <Tabs.Screen name="progress" options={{ title: 'Progresso' }} />
      <Tabs.Screen name="profile" options={{ title: 'Perfil' }} />
    </Tabs>
  );
}
```

### Navegacao programatica

```typescript
import { useRouter } from 'expo-router';

const router = useRouter();

// Navegar
router.push('/settings');

// Substituir (sem voltar)
router.replace('/(tabs)');

// Voltar
router.back();

// Com parametros
router.push({ pathname: '/quiz/[id]', params: { id: '123' } });
```

### Rota dinamica

```typescript
// app/quiz/[id].tsx
import { useLocalSearchParams } from 'expo-router';

export default function QuizScreen() {
  const { id } = useLocalSearchParams<{ id: string }>();
  return <QuizView quizId={id} />;
}
```

## Quando Buscar Docs Atualizadas

- SEMPRE antes de usar APIs novas do expo-router
- Mudancas entre versoes do Expo SDK (50, 51, 52+)
- Consultar: https://docs.expo.dev/router/introduction/
