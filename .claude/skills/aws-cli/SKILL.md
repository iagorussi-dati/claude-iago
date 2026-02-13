---
name: aws-cli
description: Skill de AWS CLI. Usar quando precisa executar comandos AWS, consultar recursos, ou gerenciar servicos via terminal.
allowed-tools: Read, Grep, Bash
---

# AWS CLI

## Documentacao Oficial

- Referencia completa: https://docs.aws.amazon.com/cli/latest/reference/
- Guia do usuario: https://docs.aws.amazon.com/cli/latest/userguide/
- Config vars: https://docs.aws.amazon.com/cli/latest/topic/config-vars.html

## Padrao de Execucao

Sempre usar:
- `--profile <nome>` (perguntar ao usuario qual profile)
- `--output json` (facilita parsing)
- `--region <regiao>` quando relevante

## Exemplos de Referencia

### Listar profiles disponiveis

```bash
aws configure list-profiles
```

### Cognito - Listar user pools

```bash
aws cognito-idp list-user-pools \
  --profile meu-profile \
  --region us-east-1 \
  --max-results 10 \
  --output json
```

### DynamoDB - Consultar tabela

```bash
aws dynamodb scan \
  --profile meu-profile \
  --table-name UserProgress \
  --region us-east-1 \
  --output json \
  --max-items 10
```

### S3 - Listar e copiar

```bash
# Listar buckets
aws s3api list-buckets --profile meu-profile --output json

# Copiar arquivo
aws s3 cp ./build s3://meu-bucket/deploy/ --recursive --profile meu-profile
```

### AppSync - Listar APIs

```bash
aws appsync list-graphql-apis \
  --profile meu-profile \
  --region us-east-1 \
  --output json
```

### CloudWatch - Buscar logs

```bash
aws logs filter-log-events \
  --profile meu-profile \
  --log-group-name /aws/lambda/minha-funcao \
  --start-time $(date -d '1 hour ago' +%s000) \
  --filter-pattern "ERROR" \
  --output json
```

## Quando Buscar Docs Atualizadas

- Comandos novos ou flags desconhecidas
- Sempre consultar: https://awscli.amazonaws.com/v2/documentation/api/latest/index.html
