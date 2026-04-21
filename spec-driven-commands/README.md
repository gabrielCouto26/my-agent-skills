# Spec-Driven Commands

Esta pasta contém uma coleção de prompts especializados para desenvolvimento orientado por especificações, seguindo um fluxo estruturado de avaliação, planejamento e validação técnica.

## Prompts Disponíveis

### 1. spec-evaluation
Prompt para atuar como revisor de requisitos de produto e regras de negócio. Avalia a qualidade, clareza, consistência e confiabilidade das informações fornecidas, identificando ambiguidades, faltas, inconsistências e suposições ocultas. Atua como uma camada de validação antes da geração de PRD, produzindo um arquivo de rascunho para revisão.

### 2. spec-prd
Prompt para gerar um Documento de Requisitos de Produto (PRD) final a partir de entrada revisada. Consolida informações confirmadas, preserva a intenção de negócio e expõe itens não resolvidos, otimizando para entrega técnica e planejamento de implementação.

### 3. tech-spec
Prompt para transformar um PRD ou especificação em um plano de execução técnica. O agente assume um papel técnico sênior apropriado e gera um plano seguro, incremental, verificável e alinhado com o codebase existente, usando o PRD como fonte primária de verdade.

### 4. tech-plan-validator
Prompt para validar um plano de implementação técnica contra o PRD, codebase atual e padrões estabelecidos. Identifica problemas de cobertura, suposições inseguras e problemas de sequenciamento, aplicando correções diretamente no arquivo do plano quando possível.

### 5. tech-implementation-validator
Prompt para auditar uma implementação completa, verificando conformidade com o PRD, plano técnico validado, arquitetura do codebase e evidências executáveis (build, lint, testes). Identifica o que está correto, parcial, faltando ou divergente, produzindo um relatório de validação.