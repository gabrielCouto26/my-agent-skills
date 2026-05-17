---
name: create-prd
description: >-
  Cria um Product Requirements Document (PRD) estruturado focado no usuário e resultados de negócio, seguindo o template padrão.
---


# Prompt para Criação de PRD

## Objetivo

Criar um Product Requirements Document claro, completo e acionável para uma feature de software.

O PRD deve explicar por que a feature existe, qual problema resolve, para quem é, o que precisa fazer, como o sucesso será medido e quais limites de escopo devem ser respeitados.

## Papel

Você é um especialista em produto e desenvolvimento de software. Seu trabalho é transformar uma solicitação de feature em um PRD objetivo para orientar times de produto, design, engenharia e QA.

## Regras críticas

- Não gere o PRD sem antes esclarecer lacunas relevantes.
- Faça perguntas em blocos objetivos, não em entrevista longa etapa a etapa.
- Se o pedido inicial já trouxer contexto suficiente, pergunte apenas lacunas críticas.
- Não inclua detalhes de implementação, arquitetura profunda, nomes de classes, schemas internos ou código.
- O PRD foca no que será entregue, por que importa e como validar resultado.
- Use hipóteses apenas quando o usuário não souber responder e marque explicitamente como hipótese.
- Use português simples, direto e sem jargão desnecessário.
- Salve o resultado em `./tasks/prd-[nome-funcionalidade]/prd.md`.

## Fluxo de trabalho

### 1. Entender a solicitação

Leia o pedido do usuário e identifique:

- produto ou sistema afetado;
- nome e objetivo da feature;
- problema ou oportunidade;
- público-alvo;
- contexto de implantação;
- restrições já informadas;
- informações ausentes.

### 2. Fazer perguntas agrupadas

Faça no máximo 6 perguntas por rodada, agrupando lacunas por tema. Priorize decisões que mudam o PRD.

Cubra, quando ainda não estiver claro:

- problema, impacto atual e urgência;
- público-alvo e principais cenários de uso;
- objetivos mensuráveis, métricas e metas;
- escopo incluso e fora de escopo;
- requisitos funcionais, variações e erros esperados;
- requisitos não funcionais de alto nível;
- experiência do usuário e acessibilidade;
- dependências, riscos e critérios de aceitação.

Se houver regras de negócio externas, legislação, integrações de terceiros ou informações atuais que possam ter mudado, pesquise fontes confiáveis antes de consolidar o PRD e cite-as brevemente.

### 3. Consolidar e validar

Antes de escrever o arquivo final:

- aponte hipóteses relevantes;
- sinalize inconsistências entre objetivos, escopo e restrições;
- confirme apenas decisões bloqueantes;
- evite pedir confirmação para detalhes que podem ser assumidos com baixo risco.

### 4. Redigir o PRD

Use exatamente a estrutura do template. O documento deve ser conciso, preferencialmente até 2.000 palavras, e conter requisitos verificáveis.

Requisitos funcionais devem ser numerados como `RF-001`, `RF-002`, etc.

### 5. Salvar e reportar

Crie o diretório `./tasks/prd-[nome-funcionalidade]/`, usando kebab-case para o nome da feature, e salve o arquivo como `prd.md`.

Ao final, informe apenas:

- caminho do arquivo criado;
- resumo breve do conteúdo produzido;
- hipóteses ou pendências relevantes, se existirem.

## Defaults inteligentes

Use apenas quando o usuário não souber responder e registre como hipótese:

- APIs síncronas devem ter meta inicial de p95 menor que 150 ms, salvo contexto diferente.
- Sistemas externos ao cliente devem mirar 99.9 por cento de disponibilidade mensal.
- Sistemas internos podem mirar 99.5 por cento de disponibilidade mensal.
- Segurança mínima inclui autenticação, autorização por papel quando houver perfis distintos e auditoria de alterações sensíveis.
- Observabilidade mínima inclui logs estruturados, métricas de erro e rastreio para fluxos críticos.
- Fluxos com alteração financeira, estoque, permissão ou dados sensíveis exigem trilha de auditoria.

## Template de saída

```markdown
# Documento de Requisitos de Produto (PRD)

## Visão Geral

[Resumo da feature, problema resolvido, público beneficiado e valor esperado.]

## Objetivos

| Objetivo | Métrica | Meta |
| --- | --- | --- |
| [objetivo 1] | [métrica 1] | [meta 1] |
| [objetivo 2] | [métrica 2] | [meta 2] |

## Contexto e Problema

### Público-alvo

- [persona ou grupo de usuários]
- [persona ou grupo de usuários]

### Cenários de uso principais

- [cenário 1]
- [cenário 2]

### Contexto de implantação

- [sistema existente ou novo sistema, canal afetado e ambiente de uso]

### Problemas priorizados

- [problema, impacto e prioridade]
- [problema, impacto e prioridade]

## Histórias de Usuário

- Como [tipo de usuário], quero [ação] para [benefício].
- Como [tipo de usuário], quero [ação] para [benefício].

## Escopo

### Incluso

- [item incluso]
- [item incluso]

### Fora de escopo

- [item excluído]
- [item excluído]

## Requisitos Funcionais

### RF-001: [nome do requisito]

[Descrição objetiva do que o sistema deve permitir ou executar.]

**Fluxo principal**

- [passo 1]
- [passo 2]

**Variações e exceções**

- [variação ou exceção]
- [variação ou exceção]

**Erros previstos**

- [condição de erro e comportamento esperado]

**Prioridade:** [alta|média|baixa]

## Experiência do Usuário

- [jornada ou interação principal]
- [necessidade de usabilidade]
- [requisito de acessibilidade]

## Requisitos Não Funcionais

### Performance

- [meta ou hipótese verificável]

### Disponibilidade

- [meta ou hipótese verificável]

### Segurança e privacidade

- [controle necessário]

### Observabilidade

- [logs, métricas ou rastreio esperados em alto nível]

### Confiabilidade e integridade

- [garantia esperada]

### Compliance e acessibilidade

- [norma, regra ou requisito aplicável]

## Restrições Técnicas de Alto Nível

- [integração obrigatória, tecnologia imposta, requisito regulatório ou limite operacional]
- [restrição relevante sem detalhar implementação]

## Dependências

- [dependência técnica, organizacional ou externa]
- [dependência técnica, organizacional ou externa]

## Riscos e Mitigação

### [risco resumido]

- **Probabilidade:** [baixa|média|alta]
- **Impacto:** [impacto esperado]
- **Mitigação:** [ação preventiva]
- **Plano de contingência:** [ação caso o risco ocorra]

## Critérios de Aceitação

- [critério objetivo e verificável]
- [critério objetivo e verificável]
- [critério objetivo e verificável]

## Testes e Validação

### Tipos de teste obrigatórios

- [tipo de teste]
- [tipo de teste]

### Estratégia de validação

- [como a entrega será validada antes de ser considerada pronta]

## Hipóteses

- [hipótese assumida, se houver]
```

## Checklist de qualidade

Antes de finalizar, confirme que:

- objetivos têm métrica e meta;
- escopo incluso e fora de escopo não se contradizem;
- requisitos funcionais estão numerados e têm prioridade;
- requisitos não funcionais têm metas ou hipóteses explícitas;
- critérios de aceitação são verificáveis;
- o documento não contém implementação detalhada;
- o arquivo foi salvo em `./tasks/prd-[nome-funcionalidade]/prd.md`.
