---
name: aws-troubleshooting
description: Skill de troubleshooting AWS. Usar quando precisa diagnosticar erros, verificar permissoes, analisar logs ou resolver problemas de servicos AWS.
allowed-tools: Read, Grep, Bash
---

# AWS Troubleshooting

## Documentacao Oficial

- IAM Troubleshooting: https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot.html
- Cognito Troubleshooting: https://docs.aws.amazon.com/cognito/latest/developerguide/troubleshooting.html
- Amplify Troubleshooting: https://docs.amplify.aws/react-native/start/troubleshooting/

## Fluxo de Diagnostico

1. Identificar o servico com problema
2. Verificar permissoes IAM (`aws sts get-caller-identity`)
3. Consultar logs no CloudWatch
4. Verificar configuracao do recurso
5. Testar conectividade/endpoint

## Exemplos de Referencia

### Verificar identidade atual

```bash
aws sts get-caller-identity --profile meu-profile --output json
```

### Verificar permissoes de uma policy

```bash
aws iam get-policy-version \
  --profile meu-profile \
  --policy-arn arn:aws:iam::123456789:policy/MinhaPolicy \
  --version-id v1 \
  --output json
```

### Cognito - Verificar user pool config

```bash
aws cognito-idp describe-user-pool \
  --profile meu-profile \
  --user-pool-id us-east-1_XXXXX \
  --region us-east-1 \
  --output json
```

### Cognito - Listar usuarios

```bash
aws cognito-idp list-users \
  --profile meu-profile \
  --user-pool-id us-east-1_XXXXX \
  --region us-east-1 \
  --limit 10 \
  --output json
```

### DynamoDB - Verificar tabela

```bash
aws dynamodb describe-table \
  --profile meu-profile \
  --table-name UserProgress \
  --region us-east-1 \
  --output json
```

### Erros comuns

| Erro | Causa provavel | Acao |
|------|---------------|------|
| AccessDeniedException | Permissao IAM faltando | Verificar policy do role/user |
| ResourceNotFoundException | Recurso nao existe na regiao | Confirmar regiao e nome |
| TokenExpired | Credenciais expiradas | `aws sso login --profile` |
| ThrottlingException | Rate limit | Adicionar retry com backoff |

## Quando Buscar Docs Atualizadas

- Erros desconhecidos ou mensagens novas
- Mudancas em servicos (Cognito v2, Amplify Gen 2, etc.)
