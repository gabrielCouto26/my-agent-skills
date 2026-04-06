---
name: create-pr
description: Use this skill when the user asks to create a Pull Request (PR) description based on the current branch. The skill will run git commands to extract changes from the target branch and format the PR text.
---

# Instructions

## Target Branch Parameter

The user can invoke the command with the name of the branch to which the PR will be merged: **`[target_branch]`**

- Example contexts: "create a pr for main" or "pr to develop".
- **`[target_branch]`** is the branch used as the base for comparisons.
- If no branch is provided, use **`main`** as the target branch.

## Mandatory Steps

1. **Get current branch:** `git branch --show-current`
2. **Get target branch:** from user input or default to `main`.
3. **List commits that are in the current branch and not in the target branch:**  
   `git log <target_branch>..HEAD --oneline` (and if useful, `git log <target_branch>..HEAD --pretty=format:"%h %s"` for messages).
4. **Summary of files changed between branches:**  
   `git diff <target_branch>...HEAD --stat` (and if necessary, excerpts from `git diff <target_branch>...HEAD` to detail changes).
5. **Generate the PR description** based on these commits and diffs, in the format below.

You should use the commits and file changes between the current branch and the target branch to create a PR template similar to this:

"""
Descrição
Cria a biblioteca compartilhada @payface-libs/aws-resources-lib para centralizar o gerenciamento de clientes AWS (DynamoDB, S3, SQS, Lambda) e integra nos serviços do monorepo. Atualiza dependências do AWS SDK e adiciona configuração do Tilt para facilitar o desenvolvimento local. PTR-88

Alterações

1. AWS Resources Lib

- Criação da biblioteca @payface-libs/aws-resources-lib em packages/aws-resources-lib para centralizar criação e gerenciamento de clientes AWS.
- Implementa AwsClientFactory com métodos para criar clientes DynamoDB, S3, SQS e Lambda com suporte a X-Ray tracing e configurações otimizadas.
- Exporta utilitários do AWS SDK (DynamoDBClient, S3Client, SQSClient, LambdaClient) e AWS Lambda Powertools (Logger, Metrics, Tracer).
- Suporte a configurações locais (LocalStack), Lambda e Kubernetes com credenciais via IRSA.
- Adiciona SetupClients com instâncias pré-configuradas dos clientes AWS.
- Inclui testes unitários para validação da biblioteca.

2. Integração nos Serviços

- Switch-Core: substitui imports diretos do AWS SDK por @payface-libs/aws-resources-lib em lambda.ts, aws-logger, metrics, tracer, product.service, SecureStorageService, SqsAdapter, DataOnly3dsService e InternalSwitchAsyncResponseSqsService.
- Backend: atualiza package.json para incluir a nova biblioteca.
- Onboarding-Service: atualiza dependências do AWS SDK.
- WebSockets-Gateway: atualiza dependências do AWS SDK.

3. Dependências e Configuração

- Atualiza versões do AWS SDK (@aws-sdk/client-dynamodb, @aws-sdk/client-s3, @aws-sdk/client-sqs, @aws-sdk/client-lambda) para versões mais recentes.
- Adiciona dependências do AWS Lambda Powertools e AWS Crypto Client.
- Atualiza pnpm-lock.yaml com as novas dependências.

Como testar

1. Instalar dependências: executar `pnpm install` na raiz do monorepo para instalar a nova biblioteca e atualizar dependências.
2. Build da biblioteca: executar `pnpm build --filter @payface-libs/aws-resources-lib` para compilar a biblioteca.
3. Testes unitários: executar `pnpm test --filter @payface-libs/aws-resources-lib` para validar a biblioteca.
"""

Important features: the PR description must be in brazilian portuguese. It must have at least these sections: "Descrição", "Alterações" and "Como testar". The output must be in code format to be easier for me to copy and paste.

**Reminder:** The description must reflect only what changed between the current branch and the target branch. Use the git commands above to get real commits and diffs before writing.

Create the PR description now based on the findings.
