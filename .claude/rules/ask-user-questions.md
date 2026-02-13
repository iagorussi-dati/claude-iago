# Regra Global: AskUserQuestion para Perguntas com Opções

## REGRA DEFINITIVA

Quando fizer perguntas ao usuário que tenham **alternativas/opções** (escolha entre A, B, C), SEMPRE use `AskUserQuestion`.

**Quando usar AskUserQuestion:**
- Perguntas com 2 ou mais opções definidas
- Escolhas múltiplas
- Qualquer decisão com alternativas claras

**Quando texto simples é OK:**
- Perguntas abertas (ex: "Qual o nome do arquivo?")
- Solicitação de descrição livre
- Confirmações simples (s/n) de ações críticas

## Por Que Usar

- Interface interativa (navegar com setas ↑↓, Enter para selecionar)
- Reduz erros de digitação
- Opções claras com descrições
- Sempre inclui opção "Não sei" ou "Outro"

## Como Usar

```javascript
AskUserQuestion({
  questions: [{
    question: "Qual plataforma você está usando?",
    header: "Plataforma",
    multiSelect: false,
    options: [
      {
        label: "iOS",
        description: "iPhone ou iPad"
      },
      {
        label: "Android",
        description: "Smartphone ou tablet Android"
      },
      {
        label: "Web",
        description: "Navegador desktop ou mobile"
      },
      {
        label: "Não sei",
        description: "Vou investigar automaticamente"
      }
    ]
  }]
})
```

## Múltiplas Perguntas

Você pode fazer até 4 perguntas de uma vez:

```javascript
AskUserQuestion({
  questions: [
    {
      question: "Qual plataforma?",
      header: "Plataforma",
      multiSelect: false,
      options: [...]
    },
    {
      question: "O problema é constante ou intermitente?",
      header: "Frequência",
      multiSelect: false,
      options: [...]
    }
  ]
})
```

## Regras de Formatação

### Header (obrigatório)
- Máximo 12 caracteres
- Exemplos: "Plataforma", "Abordagem", "AWS Profile", "Frequência"

### Label (obrigatório)
- Conciso: 1-5 palavras
- Exemplos: "Resolver direto", "iOS", "Não sei"

### Description (obrigatório)
- Explicação clara do que significa ou vai acontecer
- Exemplos: "Vou analisar o código e implementar correção", "iPhone ou iPad"

### MultiSelect
- `false` (padrão): usuário escolhe UMA opção
- `true`: usuário pode escolher múltiplas opções

## Sempre Incluir "Não Sei"

Toda pergunta de diagnóstico/investigação DEVE ter opção "Não sei":

```javascript
{
  label: "Não sei",
  description: "Vou investigar automaticamente todos os cenários"
}
```

## Exemplos por Agente

### Logger
```javascript
AskUserQuestion({
  questions: [{
    question: "Qual é o comportamento específico dos clicks?",
    header: "Comportamento",
    multiSelect: false,
    options: [
      {
        label: "Não fazem nada",
        description: "Sem resposta visual ou funcional"
      },
      {
        label: "Ação errada",
        description: "Clica no item A mas abre item B"
      },
      {
        label: "Intermitente",
        description: "Funciona algumas vezes, outras não"
      },
      {
        label: "Não sei",
        description: "Vou investigar automaticamente"
      }
    ]
  }]
})
```

### Dev
```javascript
AskUserQuestion({
  questions: [{
    question: "Como você quer que eu resolva o problema?",
    header: "Abordagem",
    multiSelect: false,
    options: [
      {
        label: "Resolver direto (Recomendado)",
        description: "Vou analisar o código e implementar a correção"
      },
      {
        label: "Debugar primeiro",
        description: "Aciono logger para investigação detalhada antes"
      }
    ]
  }]
})
```

### AWS-Specialist
```javascript
AskUserQuestion({
  questions: [{
    question: "Qual AWS profile vamos usar?",
    header: "AWS Profile",
    multiSelect: false,
    options: [
      {
        label: "poc_juan",
        description: "Profile de desenvolvimento/POC"
      },
      {
        label: "IAM-Dati-Acc",
        description: "Profile IAM principal"
      },
      {
        label: "default",
        description: "Profile padrão configurado"
      }
    ]
  }]
})
```

### Git
```javascript
AskUserQuestion({
  questions: [{
    question: "Quer criar uma branch para essa tarefa?",
    header: "Branch",
    multiSelect: false,
    options: [
      {
        label: "Sim, criar branch",
        description: "Crio branch nova e PR no final"
      },
      {
        label: "Não, commitar direto",
        description: "Commito na branch atual sem PR"
      }
    ]
  }]
})
```

## Quando NÃO Usar

- Perguntas abertas que precisam de texto livre (nome de arquivo, mensagem de commit)
- Confirmações simples de ação crítica (já tem formato específico na regra aws-safety.md)

Para texto livre, usar formato específico do contexto (ex: AWS usa formato de aviso com "Deseja prosseguir? (s/n)")
