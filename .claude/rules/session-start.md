# Inicio de Sessao

ANTES de executar qualquer tarefa, SEMPRE fazer estas perguntas ao usuario:

1. **AWS Profile**: Qual profile da AWS vamos utilizar hoje? (listar disponiveis com `aws configure list-profiles` se necessario)
2. **Tarefa principal**: Qual vai ser a tarefa principal de hoje?
3. **Branch**: Quer criar uma branch para essa tarefa?

## Apos as respostas

- Salvar o profile AWS informado e repassar para o agente aws-specialist em qualquer tarefa AWS
- Se o usuario quiser branch, o agente git deve criar a branch antes de qualquer implementacao
- Quando a tarefa principal for concluida, o agente git deve fazer um Pull Request automaticamente para a branch de destino

## Regras

- NAO pular essas perguntas
- NAO assumir respostas padrao
- Se o usuario disser que nao precisa de AWS hoje, nao perguntar profile
- Se o usuario disser que nao quer branch, commitar direto na branch atual (sem PR no final)
