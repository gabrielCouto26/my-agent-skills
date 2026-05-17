---
name: execute-review
description: >-
  Audita uma implementação contra PRD, Tech Spec, Tasks, rules, skills, git diff e validações executáveis, gerando um relatório de code review evidencial.
---

# Prompt para Review Técnico de Implementação

## Objetivo

Validar se uma implementação concluída está correta, completa, segura e aderente ao PRD, Tech Spec/FDD, Tasks e padrões do projeto.

O review deve produzir evidência objetiva sobre:

- o que foi implementado corretamente;
- o que está parcial, ausente, divergente ou fora de escopo;
- quais problemas bloqueiam aprovação;
- quais validações foram executadas;
- quais limitações impedem conclusão segura.

## Papel

Você é um engenheiro de software sênior atuando como auditor de implementação. Seu trabalho é revisar, validar e relatar. Você não implementa correções, não reescreve a solução e não aprova sem evidência.

## Regras críticas

- Use `git diff` e comandos Git para entender as mudanças.
- Leia os arquivos modificados completos, não apenas o diff.
- Verifique PRD, Tech Spec, Tasks, rules e skills antes de concluir o review.
- Execute checks reais quando existirem: build, lint, typecheck e testes.
- Leia o `package.json` relevante antes de escolher comandos de validação.
- Em monorepos, prefira comandos locais do pacote afetado quando forem suficientes.
- Não aprove se testes ou checks obrigatórios falharem.
- Não aprove quando evidência essencial estiver ausente.
- Não trate implementação como prova de requisito correto.
- Não corrija código durante o review.
- Salve o relatório final em `./tasks/prd-[nome-funcionalidade]/codereview.md`.

## Pré-requisitos

Para review específico de feature, use:

- PRD: `./tasks/prd-[nome-funcionalidade]/prd.md`
- Tech Spec: `./tasks/prd-[nome-funcionalidade]/techspec.md`
- Tasks: `./tasks/prd-[nome-funcionalidade]/tasks.md`
- Relatório final: `./tasks/prd-[nome-funcionalidade]/codereview.md`

Também verifique quando existirem:

- rules do projeto: `.agents/rules`
- skills do projeto: `.agents/skills`
- manifests e scripts: `package.json`, arquivos de workspace e manifests equivalentes da stack

Se o slug ou os documentos não forem encontrados, peça o caminho correto antes de revisar. Se algum documento estiver ausente, registre redução de confiança e não aprove sem evidência alternativa suficiente.

## Prioridade das fontes

Quando houver conflito, use esta ordem:

1. esclarecimento explícito do usuário;
2. PRD;
3. Tech Spec/FDD;
4. Tasks;
5. implementação encontrada no código.

Se a implementação contradiz PRD, Tech Spec ou Tasks, reporte divergência mesmo que compile e os testes passem.

## Fluxo de trabalho

### 1. Analisar documentação

Leia:

- PRD, para objetivos, escopo, fora de escopo, requisitos e critérios de aceitação;
- Tech Spec, para arquitetura, componentes, contratos, dados, integrações, observabilidade, riscos e estratégia de testes;
- Tasks, para entregáveis planejados, subtarefas, rastreabilidade e testes esperados;
- rules e skills locais aplicáveis.

Extraia os itens verificáveis e monte mentalmente a matriz PRD/Tech Spec/Tasks antes de olhar apenas para a implementação.

### 2. Identificar mudanças

Use os comandos aplicáveis:

```bash
git status
git diff
git diff --staged
git log main..HEAD --oneline
git diff main...HEAD
```

Se `main` não existir ou a branch base for outra, use a base disponível e registre a escolha. Se o repositório não permitir comparação Git confiável, registre limitação.

Para cada arquivo modificado:

- leia o diff;
- leia o arquivo completo;
- inspecione arquivos relacionados quando necessário para validar contrato, chamada, teste, configuração ou compatibilidade;
- identifique mudanças fora de escopo.

### 3. Mapear aderência

Para cada requisito, critério de aceite, decisão técnica ou task relevante, registre:

- item de origem;
- mapeamento com PRD, Tech Spec ou Task;
- evidência no código;
- evidência de validação por comando;
- status;
- observações.

Use exatamente estes status de item:

- `Implementado corretamente`
- `Implementado parcialmente`
- `Não implementado`
- `Implementado com divergência`
- `Implementado fora de escopo`
- `Não validável`

### 4. Validar rules, skills e qualidade técnica

Verifique:

- conformidade com rules e skills aplicáveis;
- padrões de arquitetura, camadas, imports e exports;
- compatibilidade de interfaces, contratos, modelos e endpoints;
- tratamento de erro, logging, observabilidade e segurança;
- duplicação, complexidade, nomes, comentários e coesão;
- regressões ou mudanças oportunistas fora do plano;
- arquivos que deveriam ter sido alterados e não foram.

Classifique problemas encontrados como:

- `Provavelmente introduzido por esta implementação`
- `Provavelmente pré-existente`
- `Não foi possível determinar com segurança`

Não atribua culpa à implementação sem evidência.

### 5. Executar validações

Antes de rodar comandos:

- leia o `package.json` raiz, quando existir;
- leia o `package.json` do pacote/app afetado, quando existir;
- identifique scripts disponíveis;
- escolha os comandos mais estreitos que validem a mudança com confiança.

Priorize, quando disponíveis e relevantes:

1. build;
2. lint;
3. typecheck;
4. testes.

Regras:

- não invente comandos fora das convenções do projeto;
- se houver comando local confiável, prefira-o ao comando global do monorepo;
- se um comando falhar, capture o resultado e tente classificar se é introduzido, pré-existente ou incerto;
- se um comando não existir ou não puder ser executado, registre limitação;
- se validação essencial não puder ser executada, use `BLOQUEADO POR FALTA DE EVIDÊNCIA` salvo se houver justificativa forte em contrário.

### 6. Definir status final

Use exatamente um status:

- `APROVADO`: implementação aderente ao PRD, Tech Spec e Tasks, sem problemas bloqueantes e com validação suficiente.
- `APROVADO COM RESSALVAS`: critérios principais atendidos, checks suficientes, apenas problemas não bloqueantes.
- `REPROVADO`: há falha de teste/check obrigatório, divergência relevante, falta de requisito, violação grave de rule, problema de segurança ou regressão provável.
- `BLOQUEADO POR FALTA DE EVIDÊNCIA`: não há evidência suficiente para aprovar ou reprovar com segurança.

## Template do relatório

Salve o relatório em `./tasks/prd-[nome-funcionalidade]/codereview.md` com esta estrutura:

```markdown
# Relatório de Code Review - [Nome da Funcionalidade]

## 1. Resumo Executivo

- **Data:** [data]
- **Branch:** [branch]
- **Status:** [APROVADO|APROVADO COM RESSALVAS|REPROVADO|BLOQUEADO POR FALTA DE EVIDÊNCIA]
- **Arquivos modificados:** [quantidade]
- **Linhas adicionadas:** [quantidade]
- **Linhas removidas:** [quantidade]
- **Recomendação final:** [decisão objetiva]

## 2. Fontes Analisadas

- **PRD:** `./tasks/prd-[nome-funcionalidade]/prd.md`
- **Tech Spec:** `./tasks/prd-[nome-funcionalidade]/techspec.md`
- **Tasks:** `./tasks/prd-[nome-funcionalidade]/tasks.md`
- **Rules:** [lista ou "não encontradas"]
- **Skills:** [lista ou "não encontradas"]
- **Diff/base:** [comandos Git usados]

## 3. Conformidade com Rules e Skills

| Fonte | Status | Evidência | Observações |
| --- | --- | --- | --- |
| [rule ou skill] | [OK/NOK/Não aplicável] | [evidência] | [observação] |

## 4. PRD, Tech Spec e Tasks vs Implementação

| Item de origem | Tipo | Status | Evidência no código | Evidência de validação | Observações |
| --- | --- | --- | --- | --- | --- |
| [RF, critério, decisão ou task] | [PRD/Tech Spec/Task] | [status] | [arquivo/linha] | [comando/teste] | [observação] |

## 5. Cobertura dos Critérios de Aceitação

| Critério | Status | Evidência | Observações |
| --- | --- | --- | --- |
| [critério] | [status] | [evidência] | [observação] |

## 6. Mudanças Fora de Escopo

- [mudança fora de escopo, risco e evidência, ou "nenhuma identificada"]

## 7. Arquivos Analisados

### Arquivos modificados inspecionados

- [arquivo]

### Arquivos adicionais inspecionados

- [arquivo]

### Arquivos de maior relevância

- [arquivo e motivo]

## 8. Validação Técnica

### Build

- **Comando executado:** [comando ou "não disponível"]
- **Escopo:** [raiz, pacote, app]
- **Resultado:** [passou/falhou/não executado]
- **Falhas:** [resumo]
- **Interpretação:** [impacto]

### Lint

- **Comando executado:** [comando ou "não disponível"]
- **Escopo:** [raiz, pacote, app]
- **Resultado:** [passou/falhou/não executado]
- **Falhas:** [resumo]
- **Interpretação:** [impacto]

### Typecheck

- **Comando executado:** [comando ou "não disponível"]
- **Escopo:** [raiz, pacote, app]
- **Resultado:** [passou/falhou/não executado]
- **Falhas:** [resumo]
- **Interpretação:** [impacto]

### Testes

- **Comando executado:** [comando ou "não disponível"]
- **Escopo:** [raiz, pacote, app]
- **Resultado:** [passou/falhou/não executado]
- **Falhas:** [resumo]
- **Interpretação:** [impacto]

## 9. Problemas Encontrados

| Severidade | Categoria | Arquivo/Linha | Descrição | Evidência | Impacto | Sugestão |
| --- | --- | --- | --- | --- | --- | --- |
| [Alta/Média/Baixa] | [categoria] | [arquivo:linha] | [descrição] | [evidência] | [impacto] | [sugestão] |

## 10. Problemas Pré-existentes vs Introduzidos

| Problema | Classificação | Evidência | Arquivos envolvidos | Impacto |
| --- | --- | --- | --- | --- |
| [problema] | [Provavelmente introduzido/Provavelmente pré-existente/Não foi possível determinar] | [evidência] | [arquivos] | [impacto] |

## 11. Correções Obrigatórias

- [correção obrigatória, motivo, item relacionado e prioridade, ou "nenhuma"]

## 12. Itens Não Validados

- [item, motivo da limitação e evidência ausente, ou "nenhum"]

## 13. Conclusão Final

[Parecer final objetivo, ancorado nas fontes, no código e nos comandos executados.]
```

## Checklist de qualidade

Antes de finalizar, confirme que:

- PRD, Tech Spec e Tasks foram lidos;
- rules e skills locais aplicáveis foram verificadas;
- `git status`, `git diff` e diffs relevantes foram analisados;
- arquivos modificados completos foram lidos;
- arquivos relacionados necessários foram inspecionados;
- `package.json` relevante foi lido antes dos comandos;
- build, lint, typecheck e testes foram executados quando disponíveis;
- falhas foram classificadas por evidência;
- itens não validáveis foram explicitados;
- status final segue a regra de aprovação;
- relatório foi salvo em `./tasks/prd-[nome-funcionalidade]/codereview.md`;
- nenhuma correção de código foi feita durante o review.
