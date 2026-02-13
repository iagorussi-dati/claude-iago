---
name: react-native
description: Skill de React Native e Expo. Usar quando a tarefa envolve componentes nativos, gestos, animacoes, plataforma mobile ou APIs do Expo SDK.
allowed-tools: Read, Grep, Bash, Edit, Write
---

# React Native + Expo

## Documentacao Oficial

- React Native: https://reactnative.dev/docs/getting-started
- Expo SDK: https://docs.expo.dev/versions/latest/
- Reanimated: https://docs.swmansion.com/react-native-reanimated/
- Gesture Handler: https://docs.swmansion.com/react-native-gesture-handler/

## Padroes do Projeto

- Expo managed workflow
- expo-router para navegacao
- react-native-reanimated para animacoes
- expo-sqlite para storage local
- Platform.OS check para codigo especifico

## Exemplos de Referencia

### Componente com Platform check

```typescript
import { Platform, View, Text } from 'react-native';

export function StorageInfo() {
  if (Platform.OS === 'web') {
    return <Text>Usando localStorage (web mock)</Text>;
  }
  return <Text>Usando SQLite nativo</Text>;
}
```

### Animacao com Reanimated

```typescript
import Animated, {
  useSharedValue,
  useAnimatedStyle,
  withSpring,
} from 'react-native-reanimated';

export function AnimatedCard() {
  const translateX = useSharedValue(0);

  const animatedStyle = useAnimatedStyle(() => ({
    transform: [{ translateX: translateX.value }],
  }));

  const swipeRight = () => {
    translateX.value = withSpring(300);
  };

  return (
    <Animated.View style={animatedStyle}>
      <CardContent onSwipe={swipeRight} />
    </Animated.View>
  );
}
```

### Zustand store tipado

```typescript
import { create } from 'zustand';

interface FeedState {
  currentIndex: number;
  blocks: Block[];
  nextCard: () => void;
  setBlocks: (blocks: Block[]) => void;
}

export const useFeedStore = create<FeedState>((set) => ({
  currentIndex: 0,
  blocks: [],
  nextCard: () => set((s) => ({ currentIndex: s.currentIndex + 1 })),
  setBlocks: (blocks) => set({ blocks, currentIndex: 0 }),
}));
```

## Quando Buscar Docs Atualizadas

- Novas versoes do Expo SDK
- APIs deprecadas do React Native
- Consultar: https://docs.expo.dev/versions/latest/
