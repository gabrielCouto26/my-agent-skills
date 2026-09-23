---
name: jira
description: >-
  Busca issues no Jira via MCP e salva localmente. Issues com estrutura de PRD
  vão para tasks/prd-[slug]/prd.md; demais issues vão para ENG-[NÚMERO].md.
  Opcionalmente cria branch feat/[ISSUE]-[slug] quando o usuário pedir explicitamente.
---

# Skill Jira — importar issue para o repositório

## Gatilho

Sempre que o usuário enviar `/jira [NÚMERO]` ou `/jira [PREFIXO] [NÚMERO]`:

- Sem prefixo → buscar `ENG-[NÚMERO]` (ex.: `/jira 314` → `ENG-314`)
- Com prefixo → buscar `[PREFIXO]-[NÚMERO]` (ex.: `/jira DIS 123` → `DIS-123`)
- Slug opcional → `/jira 314 prd-healthdyne-file` força a pasta `tasks/prd-healthdyne-file/`
- Branch opcional → incluir `--branch` (ou `branch`) no comando para criar `git checkout -b feat/[ISSUE]-[slug]` **apenas se a issue for PRD**

Execute o fluxo completo sem pedir confirmação intermediária.

**Padrão:** não criar branch. Só executar `git checkout -b` quando o usuário pedir explicitamente com `--branch` ou `branch`.

## Fluxo

### 1. Buscar issue no Jira

Use o MCP Atlassian (`getAccessibleAtlassianResources` → `getJiraIssue`) com:

- `issueIdOrKey`: chave da issue
- `responseContentFormat`: `markdown`
- `fields`: incluir `summary`, `description`, `status`, `issuetype`, `priority`, `assignee`, `reporter`, `parent`, `created`, `updated`

### 2. Classificar o conteúdo

**É PRD** se a descrição contiver pelo menos 4 destas seções (título ou heading):

- Visão Geral
- Objetivos
- Escopo
- Requisitos Funcionais
- Critérios de Aceitação
- Histórias de Usuário

Se o usuário passou um slug (`prd-...`), trate como PRD mesmo que a detecção falhe.

### 3. Definir destino do arquivo

#### Caso PRD → padrão `tech-prd`

1. Determinar o slug da pasta:
   - Se o usuário informou slug → usar `tasks/[slug]/` (garantir prefixo `prd-`)
   - Senão → derivar do `summary` em kebab-case com prefixo `prd-`
   - Fallback → `tasks/prd-eng-[número]/`
2. Criar o diretório `./tasks/prd-[nome-funcionalidade]/` se não existir.
3. Salvar em `./tasks/prd-[nome-funcionalidade]/prd.md`.

#### Caso não-PRD → comportamento legado

Salvar em `./ENG-[NÚMERO].md` na raiz do repositório (ou `[PREFIXO]-[NÚMERO].md` quando prefixo informado).

### 4. Formato do arquivo

#### Para `prd.md`

```markdown
# Documento de Requisitos de Produto (PRD)

> **Jira:** [ENG-314](https://apalyrx.atlassian.net/browse/ENG-314)
> **Status:** Em andamento | **Epic:** ENG-291 — Rx Transfer Integrations
> **Importado em:** YYYY-MM-DD

[descrição da issue, preservando seções e formatação markdown]
```

- Use o título padrão do PRD (`# Documento de Requisitos de Produto (PRD)`), não o summary da issue.
- Preserve a estrutura original (RF-001, tabelas, listas).
- Não reescreva nem resuma o conteúdo importado.

#### Para `ENG-[NÚMERO].md`

Manter o formato com metadados Jira no topo e a descrição completa abaixo:

```markdown
# ENG-[NÚMERO]: [summary da issue]

**Jira:** https://apalyrx.atlassian.net/browse/ENG-[NÚMERO]
**Tipo:** ...
**Status:** ...
...

[descrição da issue]
```

### 5. Regras de slug (kebab-case)

Ao derivar automaticamente do `summary`:

1. Remover prefixos entre colchetes (`[RxTransfer-6]`)
2. Converter para minúsculas
3. Substituir espaços e underscores por hífen
4. Remover caracteres especiais
5. Prefixar com `prd-`
6. Limitar a ~50 caracteres; se truncar, preferir palavras completas

Exemplo:
`[RxTransfer-6] Add support for prescription file input and send to HealthDyne`
→ `prd-healthdyne-prescription-file`

### 6. Criar branch (opcional, apenas PRD)

Executar **somente** quando o usuário pedir explicitamente (`--branch` ou `branch` no comando) **e** a issue tiver sido classificada como PRD.

1. Derivar o sufixo da branch a partir do slug da pasta, removendo o prefixo `prd-`:
   - Pasta `tasks/prd-healthdyne-example/` → sufixo `healthdyne-example`
2. Montar o nome da branch:
   - `feat/[ISSUE_KEY]-[sufixo-sem-prd-]`
   - Exemplo: issue `ENG-314`, pasta `prd-healthdyne-example` → `feat/ENG-314-healthdyne-example`
3. Executar na raiz do repositório:
   ```bash
   git checkout -b feat/[ISSUE_KEY]-[sufixo-sem-prd-]
   ```
4. Se a branch já existir localmente, informar o conflito e **não** sobrescrever; seguir na branch atual.
5. Se a issue **não** for PRD, ignorar o pedido de branch e informar que branch só é criada para PRDs.

**Não criar branch** quando:
- o usuário não incluiu `--branch` ou `branch` no comando;
- a issue não foi classificada como PRD;
- o comando `git checkout -b` falhar por outro motivo (reportar o erro).

### 7. Conflitos

- Se `tasks/prd-[slug]/prd.md` já existir, **sobrescrever** (importação do Jira é fonte de verdade).
- Se a pasta existir com outros arquivos (`techspec.md`, `tasks.md`), **não alterar** esses arquivos.

### 8. Reportar ao usuário

Informar:

- chave Jira buscada
- caminho do arquivo criado
- se foi classificado como PRD ou issue genérica
- slug usado (e se foi automático ou informado pelo usuário)
- se branch foi criada (nome da branch) ou se foi ignorada (e por quê)

## Exemplos

| Comando | Resultado |
| --- | --- |
| `/jira 314` | `tasks/prd-healthdyne-prescription-file/prd.md` (sem branch) |
| `/jira 314 prd-healthdyne-file` | `tasks/prd-healthdyne-file/prd.md` (sem branch) |
| `/jira 314 --branch` | `tasks/prd-healthdyne-prescription-file/prd.md` + branch `feat/ENG-314-healthdyne-prescription-file` |
| `/jira 310 prd-healthdyne-example --branch` | `tasks/prd-healthdyne-example/prd.md` + branch `feat/ENG-310-healthdyne-example` |
| `/jira 999` (issue sem PRD) | `ENG-999.md` (sem branch, mesmo com `--branch`) |
| `/jira DIS 123` | `DIS-123.md` ou PRD em `tasks/prd-.../prd.md` se aplicável |

## Integração com `tech-prd`

- O destino PRD segue o mesmo padrão: `./tasks/prd-[nome-funcionalidade]/prd.md`
- Esta skill **importa** PRDs já escritos no Jira; a skill `tech-prd` **cria** PRDs novos a partir de conversa
- Não rodar `tech-prd` em cima de um import — o conteúdo já veio pronto do Jira
