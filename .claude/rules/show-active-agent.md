# Exibir Agente Ativo

Sempre que um agente for acionado, exibir no inicio da resposta qual agente esta sendo utilizado no formato:

```
[agente: nome] descricao curta do que vai fazer
```

Exemplos:
```
[agente: dev] implementando correcao no sync-service
[agente: speed] ajustando texto do botao de login
[agente: logger] investigando erro de sync no iOS
[agente: aws-specialist] consultando user pool no Cognito
[agente: git] commitando mudancas do feed
[agente: orchestrator] analisando solicitacao para rotear
```

Isso vale para TODOS os agentes, incluindo o orchestrator. Se trocar de agente durante a tarefa, exibir novamente.
