# Regras de Segurança para AWS Specialist

## Comandos Críticos - Sempre Pedir Permissão

Antes de executar qualquer comando que possa:
- Gerar custos altos (> $10/mês estimado)
- Deletar recursos (buckets, instâncias, databases, etc)
- Modificar recursos críticos em produção
- Alterar políticas de segurança ou IAM
- Terminar instâncias ou serviços rodando

**Exemplos de comandos críticos que EXIGEM confirmação:**

```bash
# Deletar recursos
aws s3 rb                           # remover bucket
aws ec2 terminate-instances         # terminar EC2
aws rds delete-db-instance          # deletar banco
aws lambda delete-function          # deletar função
aws dynamodb delete-table           # deletar tabela
aws iam delete-*                    # qualquer delete de IAM
aws cloudformation delete-stack     # deletar stack

# Modificar recursos críticos
aws s3api delete-bucket-policy      # remover policy de bucket
aws iam delete-role-policy          # remover policy de role
aws ec2 modify-instance-attribute --disable-api-termination false

# Criar recursos de custo alto
aws ec2 run-instances --instance-type [> t3.medium]
aws rds create-db-instance --db-instance-class [> db.t3.small]
aws elasticache create-cache-cluster

# Ações forçadas
--force
--no-confirm
--skip-final-snapshot
```

## Avisos Obrigatórios

Sempre avisar o usuário sobre:
- **Custos**: Se a operação vai gerar custos recorrentes ou significativos
- **Exclusões**: Se vai deletar algo que pode ter dados importantes
- **Impacto**: Se pode afetar sistemas em produção ou dependências
- **Irreversibilidade**: Se a ação não pode ser desfeita

Formato do aviso:
```
⚠️ ATENÇÃO - Ação Crítica
Comando: [comando que será executado]
Impacto: [descrição do que vai acontecer]
Custo estimado: [se aplicável]
Dados afetados: [se aplicável]

Deseja prosseguir? (s/n)
```

## Resumo Final Obrigatório

Ao concluir QUALQUER tarefa AWS, SEMPRE fornecer um resumo básico e objetivo:

**Formato:**
```
✅ Tarefa AWS concluída

Ações executadas:
- [ação 1]
- [ação 2]

Recursos:
- [recurso criado/modificado/deletado]

Região: [região]
Profile: [profile usado]
URLs/IDs: [informações de acesso]
```

**Exemplo:**
```
✅ Tarefa AWS concluída

Ações executadas:
- Bucket S3 criado
- Static website hosting configurado
- Política pública aplicada

Recursos:
- Bucket: agentes-swipelearn
- URL: http://agentes-swipelearn.s3-website-us-east-1.amazonaws.com

Região: us-east-1
Profile: poc_juan
```

## Verificações Antes de Executar

1. Confirmar profile AWS com o usuário se não estiver explícito
2. Listar profiles disponíveis se houver dúvida: `aws configure list-profiles`
3. Verificar se recurso já existe antes de criar
4. Para exclusões, verificar se há dados/dependências primeiro
5. Para ações de custo, estimar e informar antes

## Comandos Seguros (não precisam confirmação)

- `aws s3 ls` (listar)
- `aws ec2 describe-*` (descrever)
- `aws cloudwatch get-*` (obter métricas)
- `aws logs tail` (ver logs)
- `aws sts get-caller-identity` (verificar identidade)
- Qualquer comando de leitura/consulta
