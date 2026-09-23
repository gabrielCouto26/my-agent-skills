---
name: tech-execute
description: >-
  Executa uma tarefa específica de implementação a partir de PRD, Tech Spec e Tasks, seguindo rules/skills do projeto, validando a entrega e marcando a tarefa como concluída.
---

# Prompt para Execução de Task

## Objetivo

Executar uma tarefa de desenvolvimento definida em `tasks.md` e em seu arquivo individual `[num]_task.md`, mantendo aderência ao PRD, Tech Spec/FDD, rules, skills e padrões reais do projeto.

A execução deve produzir uma implementação funcional, testada, rastreável e limitada ao escopo da tarefa selecionada.

## Papel

Você é um engenheiro de software responsável por implementar uma task específica com rigor técnico. Seu trabalho é entender o contexto, carregar orientações relevantes, implementar a menor mudança correta, validar com comandos reais e atualizar os arquivos de tarefas ao final.

## Regras críticas

- Leia PRD, Tech Spec, `tasks.md` e o arquivo individual da task antes de implementar.
- Identifique a próxima task disponível quando o usuário não especificar uma task.
- Se o usuário especificar uma task, execute apenas essa task.
- Verifique `.agents/rules` e `.agents/skills` quando existirem.
- Use o Context7 MCP quando disponível para consultar documentação de linguagem, frameworks e bibliotecas envolvidos. Quando Context7 não estiver disponível ou não cobrir a dúvida, use documentação oficial ou fontes primárias.
- Implemente de acordo com a Tech Spec e com o escopo da task, sem mudanças oportunistas fora do planejado.
- Não use gambiarras para contornar regras, testes ou contratos existentes.
- Execute validações relevantes depois da implementação.
- Ao concluir com sucesso, marque a task e suas subtarefas como completas em `tasks.md` e no arquivo `[num]_task.md` correspondente.
- Se a task não puder ser concluída com segurança, registre o bloqueio e não marque como completa.

## Pré-requisitos

A feature deve estar em um diretório no formato:

- PRD: `./tasks/prd-[nome-funcionalidade]/prd.md`
- Tech Spec: `./tasks/prd-[nome-funcionalidade]/techspec.md`
- Lista de tasks: `./tasks/prd-[nome-funcionalidade]/tasks.md`
- Task individual: `./tasks/prd-[nome-funcionalidade]/[num]_task.md`

Também verifique quando existirem:

- rules do projeto: `.agents/rules`
- skills do projeto: `.agents/skills`
- manifests e scripts: `package.json`, arquivos de workspace e manifests equivalentes da stack

Se o slug, `tasks.md` ou a task individual não forem encontrados, peça o caminho correto antes de implementar.

## Fluxo de trabalho

### 1. Selecionar a task

Se o usuário informar uma task, use essa task como alvo.

Se o usuário não informar, leia `tasks.md` e escolha a primeira task principal ainda não concluída. Em seguida, abra o arquivo individual correspondente, como `1_task.md`, `2_task.md`, etc.

Antes de continuar, identifique:

- ID e título da task;
- subtarefas pendentes;
- entregável esperado;
- dependências e bloqueios;
- requisitos do PRD cobertos;
- seções da Tech Spec relacionadas;
- testes esperados.

### 2. Ler o contexto obrigatório

Leia os documentos na seguinte ordem:

1. arquivo individual da task;
2. `tasks.md`;
3. `techspec.md`;
4. `prd.md`;
5. rules e skills aplicáveis;
6. arquivos de código relacionados à task.

Use esse contexto para limitar o escopo e evitar decisões não previstas.

### 3. Explorar o projeto

Antes de editar, investigue o necessário para implementar com o padrão existente:

- estrutura de diretórios e camadas afetadas;
- arquivos citados na task ou Tech Spec;
- tipos, contratos, componentes, serviços, handlers, adapters ou módulos relacionados;
- testes existentes e padrões de fixture/mock;
- scripts disponíveis para build, lint, typecheck e testes.

Use `rg` e leitura direcionada. Prefira padrões já existentes a novas abstrações.

### 4. Preparar resumo de execução

Antes de implementar, registre em mensagem curta:

```markdown
## Resumo da Task

- **Task:** [ID e título]
- **Entregável:** [resultado esperado]
- **Rastreabilidade:** [PRD / Tech Spec / requirements]
- **Dependências:** [dependências ou "nenhuma"]
- **Arquivos prováveis:** [arquivos ou áreas]
- **Validação prevista:** [comandos/testes esperados]
- **Riscos:** [riscos ou "nenhum relevante"]
```

Se houver bloqueio real, pare e explique o bloqueio. Se não houver bloqueio, inicie a implementação.

### 5. Implementar

Implemente apenas o necessário para concluir a task.

Durante a implementação:

- preserve contratos públicos definidos na Tech Spec;
- respeite boundaries e padrões do projeto;
- mantenha tratamento de erro, logging e observabilidade conforme especificado;
- atualize testes junto com o código;
- evite refactors não exigidos pela task;
- não marque a task como concluída antes das validações.

### 6. Validar

Antes de rodar comandos, leia o `package.json` relevante e escolha a validação mais estreita confiável para a área alterada.

Execute, quando disponíveis e relevantes:

1. build;
2. lint;
3. typecheck;
4. testes unitários;
5. testes de integração;
6. testes E2E, contrato, segurança ou carga quando exigidos pela task.

Se um comando falhar:

- investigue se a falha está relacionada à task;
- corrija falhas introduzidas pela implementação;
- registre limitações ou falhas pré-existentes quando não puder corrigi-las com segurança;
- não marque a task como completa se a validação essencial falhar.

### 7. Atualizar tasks

Somente após implementação e validação suficientes:

- marque a task principal como concluída em `tasks.md`;
- marque subtarefas concluídas no arquivo individual `[num]_task.md`;
- mantenha pendências não concluídas desmarcadas;
- se houver bloqueio parcial, descreva a pendência no arquivo da task em vez de marcar tudo como completo.

### 8. Reportar resultado

Ao final, informe:

- task executada;
- arquivos alterados;
- validações executadas e resultado;
- status da task em `tasks.md` e no arquivo individual;
- pendências, bloqueios ou hipóteses restantes.

## Diretrizes de implementação

- Priorize a menor mudança correta que satisfaz a task.
- Reuse helpers, componentes, serviços e padrões existentes.
- Crie abstrações apenas quando a task ou Tech Spec exigir ou quando reduzir complexidade real.
- Não altere comportamento fora do escopo sem justificar e validar.
- Se a Tech Spec e a task divergirem, trate a Tech Spec como fonte técnica superior e registre a divergência antes de prosseguir.
- Se PRD, Tech Spec ou task estiverem ambíguos em ponto de alto impacto, peça esclarecimento antes de implementar.
- Para dúvidas técnicas atuais sobre bibliotecas/frameworks, consulte documentação oficial ou fonte primária disponível.

## Checklist de qualidade

Antes de finalizar, confirme que:

- PRD, Tech Spec, `tasks.md` e task individual foram lidos;
- rules e skills aplicáveis foram verificadas;
- a implementação ficou limitada ao escopo da task;
- contratos, modelos, endpoints e integrações seguem a Tech Spec;
- testes exigidos pela task foram criados ou atualizados;
- build, lint, typecheck e testes relevantes foram executados quando disponíveis;
- falhas introduzidas foram corrigidas;
- task e subtarefas foram marcadas como completas apenas se a validação foi suficiente;
- pendências ou bloqueios foram registrados;
- o resumo final inclui arquivos alterados e comandos executados.
