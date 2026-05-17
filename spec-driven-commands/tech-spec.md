---
name: create-techspec
description: >-
  Produz uma Especificação Técnica (Tech Spec) clara e pronta para implementação a partir de um PRD, definindo a arquitetura do sistema.
---


# Prompt para Criação de FDD e Tech Spec

## Objetivo

Criar um Feature Design Doc técnico, compatível com o fluxo de Tech Spec do projeto, a partir de um PRD existente.

O documento deve especificar como implementar a feature com arquitetura, componentes, fluxos, contratos, dados, integrações, tratamento de erros, observabilidade, testes, riscos e sequência de desenvolvimento.

O arquivo final deve ser salvo como `./tasks/prd-[nome-funcionalidade]/techspec.md`.

## Papel

Você é um especialista em arquitetura de software e especificações técnicas. Seu trabalho é traduzir o PRD em uma especificação pronta para orientar implementação por outro agente ou desenvolvedor.

## Regras críticas

- Não implemente código. O objetivo é produzir a especificação técnica.
- Não gere a Tech Spec sem ler o PRD.
- Explore o projeto antes de fazer perguntas técnicas.
- Não pule análise de arquivos, módulos, padrões, testes, configurações e pontos de integração.
- Faça perguntas em blocos objetivos, não em entrevista longa etapa a etapa.
- Se o PRD e o repositório já responderem parte das dúvidas, pergunte apenas lacunas críticas.
- Use o template de saída definido neste arquivo.
- Salve o resultado em `./tasks/prd-[nome-funcionalidade]/techspec.md`.

## Pré-requisitos

- Receber ou inferir o slug da feature.
- Confirmar que existe `./tasks/prd-[nome-funcionalidade]/prd.md`.
- Se o PRD não existir, peça o caminho correto ou solicite que o PRD seja criado antes.

## Fluxo de trabalho

### 1. Ler o PRD

Extraia:

- objetivo da feature;
- requisitos funcionais;
- requisitos não funcionais;
- restrições de negócio e alto nível;
- critérios de aceitação;
- riscos e dependências.

Não repita a narrativa de negócio na Tech Spec além do mínimo necessário para contextualizar decisões técnicas.

### 2. Explorar o projeto

Antes de perguntar ao usuário, investigue:

- estrutura de diretórios e entrypoints;
- linguagem, framework, gerenciador de pacotes e comandos disponíveis;
- módulos, serviços, handlers, componentes, jobs ou adapters relacionados;
- modelos de dados, migrations, schemas e contratos existentes;
- autenticação, autorização, middleware e tratamento de erros;
- observabilidade, logging, métricas e tracing já usados;
- testes existentes e padrões de fixture/mock;
- arquivos em `.agents/rules` e `.agents/skills`, quando existirem.

Use `rg` e leitura direcionada de arquivos. Para decisões dependentes de tecnologia ou serviços externos, priorize documentação oficial ou fontes primárias.

### 3. Analisar padrões e alternativas

Defina a abordagem técnica com base no repositório:

- reutilizar componentes, bibliotecas e padrões existentes sempre que viável;
- justificar qualquer desvio relevante;
- comparar reuso versus construção customizada quando houver trade-off real;
- explicitar dependências bloqueantes;
- registrar riscos técnicos e mitigação.

### 4. Fazer perguntas agrupadas

Faça no máximo 6 perguntas por rodada, somente sobre decisões que não possam ser inferidas com segurança.

Priorize:

- limites de domínio e propriedade dos módulos;
- contratos públicos e compatibilidade;
- regras de persistência, concorrência e idempotência;
- integrações externas, timeouts, retries e fallback;
- metas de performance, disponibilidade e observabilidade;
- cenários críticos de teste e aceite técnico.

Quando uma resposta não vier, use hipótese explícita apenas se o risco for baixo. Para decisões de alto impacto, peça confirmação.

### 5. Redigir a Tech Spec

Use o template de saída sem mudar os títulos principais. Mantenha o documento conciso, preferencialmente até 2.500 palavras.

Inclua exemplos curtos de interfaces ou contratos apenas quando ajudarem a remover ambiguidade. Evite blocos longos de código.

### 6. Salvar e reportar

Crie ou reutilize o diretório `./tasks/prd-[nome-funcionalidade]/` e salve o arquivo como `techspec.md`.

Ao final, informe:

- caminho do arquivo criado;
- resumo breve da abordagem técnica;
- hipóteses, riscos ou pendências relevantes.

## Template de saída

```markdown
# Especificação Técnica

## Resumo Executivo

[Visão técnica curta da solução, decisões principais e estratégia de implementação.]

## Arquitetura do Sistema

### Visão Geral dos Componentes

- [componente novo ou alterado]: [responsabilidade]
- [componente novo ou alterado]: [responsabilidade]

### Fluxo de Dados

- [passo técnico principal]
- [passo técnico principal]

### Encaixe no Sistema Existente

- [módulo, serviço, camada ou integração afetada]
- [padrão existente que será seguido]

## Design de Implementação

### Interfaces Principais

\```text
[assinatura curta de serviço, função, endpoint, evento ou contrato público]
\```

### Modelos de Dados

- [entidade, estrutura, tabela, campo ou alteração de modelo]
- [regra de validação ou persistência]

### Endpoints de API

- `[método] [caminho]`: [descrição, autenticação, entrada e saída esperadas]
- `[método] [caminho]`: [descrição, autenticação, entrada e saída esperadas]

### Contratos Públicos

#### [nome do contrato]

- **Tipo:** [método|endpoint|fila|stream|sdk|evento]
- **Assinatura ou rota:** [assinatura ou rota]
- **Campos de entrada:** [campos e semântica]
- **Campos de saída:** [campos e semântica]
- **Headers ou metadados:** [quando aplicável]
- **Compatibilidade:** [garantia de versão ou impacto esperado]
- **Limites:** [timeout, tamanho, taxa ou volume esperado]

## Pontos de Integração

- [serviço, API, fila, banco, cache ou sistema externo]
- [autenticação, timeout, retry, fallback ou modo de falha relevante]

## Erros, Exceções e Fallback

| Condição | Tratamento | Observação |
| --- | --- | --- |
| [erro ou condição] | [comportamento esperado] | [nota] |
| [erro ou condição] | [comportamento esperado] | [nota] |

### Estratégias de Resiliência

- [timeout, retry, backoff, idempotência, circuit breaker ou compensação]

### Invariantes

- [condição que deve permanecer verdadeira]
- [condição que deve permanecer verdadeira]

## Abordagem de Testes

### Testes de Unidade

- [componentes e cenários]

### Testes de Integração

- [integrações e dados de teste]

### Testes E2E

- [fluxos ponta a ponta, quando aplicável]

### Testes de Contrato, Segurança ou Carga

- [cenários obrigatórios, quando aplicável]

## Sequenciamento de Desenvolvimento

### Ordem de Construção

1. [primeiro entregável técnico e motivo]
2. [segundo entregável técnico e dependência]
3. [integração, validação e ajustes finais]

### Dependências Técnicas

- [dependência bloqueante]
- [dependência bloqueante]

## Monitoramento e Observabilidade

### Métricas

- [métrica, rótulos principais e objetivo]

### Logs

- [evento, nível e campos essenciais]

### Tracing

- [span ou correlação necessária]

### Dashboards e Alertas

- [painel ou alerta mínimo]

## Considerações Técnicas

### Decisões Principais

- [decisão, justificativa e trade-off]
- [decisão, justificativa e trade-off]

### Alternativas Rejeitadas

- [alternativa e motivo]

### Riscos Conhecidos

- [risco, impacto e mitigação]

### Conformidade com Rules

- [rule aplicável encontrada em `.agents/rules`, ou "não encontrada"]

### Conformidade com Skills

- [skill aplicável encontrada em `.agents/skills`, ou "não encontrada"]

### Arquivos Relevantes e Dependentes

- [arquivo ou diretório relevante]
- [arquivo ou diretório relevante]

## Hipóteses

- [hipótese técnica assumida, se houver]
```

## Checklist de qualidade

Antes de finalizar, confirme que:

- o PRD foi lido;
- o projeto foi explorado antes das perguntas;
- regras e skills locais aplicáveis foram verificadas;
- a Tech Spec descreve como implementar sem executar a implementação;
- componentes novos e alterados estão listados;
- contratos, dados, integrações e erros estão claros;
- testes cobrem unidade, integração e E2E quando aplicável;
- observabilidade inclui métricas, logs e tracing quando relevante;
- riscos, decisões e arquivos relevantes estão documentados;
- o arquivo foi salvo em `./tasks/prd-[nome-funcionalidade]/techspec.md`.
