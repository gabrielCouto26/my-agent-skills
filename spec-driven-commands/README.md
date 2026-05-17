# Spec-Driven Commands

Esta pasta contém prompts para um fluxo de desenvolvimento orientado por especificação, cobrindo definição de produto, desenho técnico, decomposição em tarefas e revisão final da implementação.

## Fluxo Atual

O fluxo atualizado desta pasta é:

1. `tech-prd.md`
2. `tech-spec.md`
3. `tech-tasks.md`
4. `tech-review.md`

Cada prompt gera ou consome artefatos dentro de `./tasks/prd-[nome-funcionalidade]/`, mantendo rastreabilidade entre requisito, desenho técnico, plano de execução e validação final.

## Prompts Disponíveis

### 1. `tech-prd.md`

Prompt de criação de PRD.

- Nome interno: `create-prd`
- Objetivo: transformar uma solicitação de feature em um Product Requirements Document claro, completo e acionável
- Saída esperada: `./tasks/prd-[nome-funcionalidade]/prd.md`
- Foco: problema, público-alvo, escopo, requisitos funcionais, requisitos não funcionais, riscos, critérios de aceitação e hipóteses

### 2. `tech-spec.md`

Prompt de criação de Tech Spec / FDD.

- Nome interno: `create-techspec`
- Objetivo: traduzir o PRD em uma especificação técnica pronta para implementação
- Entrada principal: `./tasks/prd-[nome-funcionalidade]/prd.md`
- Saída esperada: `./tasks/prd-[nome-funcionalidade]/techspec.md`
- Foco: arquitetura, componentes, fluxo de dados, contratos, integrações, tratamento de erros, observabilidade, testes, riscos e sequência de desenvolvimento

### 3. `tech-tasks.md`

Prompt de decomposição em tarefas.

- Nome interno: `create-tasks`
- Objetivo: quebrar PRD e Tech Spec em tarefas pequenas, sequenciadas, testáveis e rastreáveis
- Entradas principais:
  - `./tasks/prd-[nome-funcionalidade]/prd.md`
  - `./tasks/prd-[nome-funcionalidade]/techspec.md`
- Saídas esperadas:
  - `./tasks/prd-[nome-funcionalidade]/tasks.md`
  - `./tasks/prd-[nome-funcionalidade]/1_task.md`, `2_task.md`, etc.
- Observação importante: antes de gerar os arquivos, o prompt exige apresentar uma proposta high-level e aguardar aprovação explícita do usuário

### 4. `tech-review.md`

Prompt de review técnico da implementação.

- Nome interno: `execute-review`
- Objetivo: auditar a implementação concluída contra PRD, Tech Spec, Tasks, rules, skills, diff Git e validações executáveis
- Entradas principais:
  - `./tasks/prd-[nome-funcionalidade]/prd.md`
  - `./tasks/prd-[nome-funcionalidade]/techspec.md`
  - `./tasks/prd-[nome-funcionalidade]/tasks.md`
- Saída esperada: `./tasks/prd-[nome-funcionalidade]/codereview.md`
- Foco: aderência ao escopo, cobertura dos critérios de aceitação, conformidade técnica, mudanças fora de escopo e evidência por build, lint, typecheck e testes

## Estrutura de Artefatos

Para cada funcionalidade, o fluxo espera um diretório no formato:

```text
./tasks/prd-[nome-funcionalidade]/
```

Arquivos gerados ao longo do processo:

- `prd.md`
- `techspec.md`
- `tasks.md`
- `[n]_task.md`
- `codereview.md`

## Observações

- Os nomes descritos neste README refletem os arquivos atualmente existentes na pasta.
- O fluxo anterior com prompts como `spec-evaluation`, `spec-prd`, `tech-plan-validator` e `tech-implementation-validator` não corresponde mais ao conteúdo atual deste diretório.
