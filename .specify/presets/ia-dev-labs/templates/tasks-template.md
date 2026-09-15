---
description: "Task list template for feature implementation and verification"
---

<!-- IA-dev Labs formatting: prosa em Markdown não usa hard wrap; mantenha cada parágrafo em uma única linha física e deixe a quebra visual para o editor. -->

# Tasks: [FEATURE NAME]

**Input**: Design documents from `/specs/[###-feature-name]/`

**Prerequisites**: `plan.md` (required), `spec.md` (required), and other artifacts when produced

**Tests**: Testes e verificações NÃO são opcionais quando a Constitution, a spec ou o plan identificarem comportamentos críticos. O nível de teste deve seguir a estratégia definida no `plan.md`.

**TDD**: NÃO é obrigatório por padrão. Só ordenar “test first” quando houver decisão explícita no projeto, na feature ou no plano.

**Organization**: Tasks devem ser agrupadas por incremento/user story quando isso permitir implementação e validação independentes. Dependências reais prevalecem sobre uma estrutura cerimonial.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: pode executar em paralelo porque não há dependência nem conflito de arquivo;
- **[Story]**: identifica a user story relacionada, por exemplo `US1`;
- toda task de implementação DEVE citar caminhos de arquivos concretos;
- tasks de verificação DEVEM indicar comando, suíte, cenário ou evidência esperada quando aplicável.

## Path Conventions

Use exclusivamente os caminhos definidos no `plan.md` e na codebase real.

NÃO invente `models/`, `services/`, `repositories/`, `adapters/`, `controllers/` ou qualquer outra camada apenas porque ela aparece em um padrão conhecido.

<!--
============================================================================ IMPORTANT: As tasks abaixo são EXEMPLOS ESTRUTURAIS.

O $speckit-tasks DEVE substituí-las por tasks reais derivadas de:
  - user stories e requisitos da spec;
  - comportamentos críticos;
  - decisões do plan;
  - data-model e contracts quando existirem;
  - estratégia de testes definida no plan;
  - Constitution do projeto.

NÃO manter sample tasks no tasks.md final. ============================================================================
-->

## Phase 1: Setup

**Purpose**: apenas preparação realmente necessária para esta feature.

- [ ] T001 [Exemplo: ajustar configuração ou estrutura já aprovada em caminho concreto]

**Checkpoint**: preparação mínima concluída.

---

## Phase 2: Foundational

**Purpose**: pré-requisitos que realmente bloqueiam mais de uma user story.

<!--
Não criar uma fase foundational artificial. Se não houver pré-requisito compartilhado, remover esta fase.
-->

- [ ] T002 [Exemplo: implementar contrato ou infraestrutura compartilhada realmente necessária]

**Checkpoint**: pré-requisitos compartilhados concluídos.

---

## Phase 3: User Story 1 - [Title] (Priority: P1)

**Goal**: [valor entregue]

**Independent Test**: [como provar o comportamento observável]

### Verification for User Story 1

<!--
Criar apenas os níveis de teste definidos no plan. Testes podem ser implementados antes ou junto da implementação; TDD não é obrigatório.
-->

- [ ] T003 [P] [US1] [Teste de domínio/integração/componente/contrato conforme plan, com caminho concreto]
- [ ] T004 [US1] [Evidência E2E/browser/manual controlada quando aplicável]

### Implementation for User Story 1

- [ ] T005 [US1] [Mudança concreta em caminho real]
- [ ] T006 [US1] [Mudança concreta em caminho real]

### Completion for User Story 1

- [ ] T007 [US1] Executar verificações relevantes e registrar o resultado real
- [ ] T008 [US1] Confirmar acceptance scenarios e casos negativos aplicáveis

**Checkpoint**: US1 entrega valor e possui evidência suficiente de funcionamento.

---

## Phase 4: User Story 2 - [Title] (Priority: P2)

**Goal**: [valor entregue]

**Independent Test**: [como provar o comportamento observável]

### Verification for User Story 2

- [ ] T009 [P] [US2] [Teste conforme estratégia do plan]

### Implementation for User Story 2

- [ ] T010 [US2] [Mudança concreta em caminho real]

### Completion for User Story 2

- [ ] T011 [US2] Executar verificações relevantes e registrar o resultado real

**Checkpoint**: US2 funciona sem regredir US1.

---

[Adicionar fases conforme as user stories reais]

---

## Final Phase: Cross-Cutting Verification & Cleanup

**Purpose**: somente itens transversais realmente necessários.

- [ ] TXXX Executar testes novos e existentes relevantes
- [ ] TXXX Executar análise estática/lint/typecheck aplicáveis
- [ ] TXXX Verificar fluxos visíveis críticos em browser/interface quando aplicável
- [ ] TXXX Verificar casos negativos de autorização quando aplicável
- [ ] TXXX Validar contratos externos localmente e chamadas reais controladas quando previstas
- [ ] TXXX Confirmar que nenhum teste foi alterado apenas para aceitar comportamento incorreto
- [ ] TXXX Registrar comandos executados, resultados reais e desvios conhecidos
- [ ] TXXX Atualizar documentação somente quando a mudança exigir

---

## Dependencies & Execution Order

### Phase Dependencies

- Setup só existe quando houver preparação necessária.
- Foundational só existe quando houver bloqueadores realmente compartilhados.
- User stories devem respeitar dependências reais do produto e da arquitetura.
- Verificação não é uma etapa decorativa final: cada história deve ter evidência suficiente antes de
ser considerada concluída.

### Within Each User Story

A ordem deve seguir as dependências reais da feature.

NÃO existe regra constitucional “models before services” ou “tests always first”.

Quando o plan exigir um teste como prova de uma propriedade crítica, a task correspondente DEVE existir e ser concluída antes de declarar a história pronta.

### Parallel Opportunities

Marque `[P]` somente quando:

- arquivos diferentes não geram conflito;
- nenhuma task depende do resultado da outra;
- a execução paralela não ameaça consistência ou compreensão do trabalho.

---

## Notes

- Tasks vagas como “implementar backend” ou “adicionar testes” NÃO são aceitáveis.
- Tasks de implementação devem citar caminhos concretos.
- Tasks de verificação devem dizer o que será provado.
- Não inventar regras de negócio para completar uma task.
- Não criar arquitetura preventiva durante decomposição.
- Correções de bugs devem incluir teste de regressão quando tecnicamente viável.
- Flakiness recorrente é defeito de engenharia.
- O agente NÃO DEVE declarar sucesso sem evidência real da execução correspondente.
