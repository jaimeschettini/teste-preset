---
description: "Tarefas de implementação e verificação para gerenciamento de tarefas"
---

# Tasks: Gerenciamento de tarefas

**Input**: Documentos de design em `/specs/001-task-management/`

**Prerequisites**: `plan.md`, `spec.md`, `data-model.md`, `contracts/http-api.md`, `research.md` e `quickstart.md`

**Tests**: As tarefas de teste são obrigatórias porque a Constitution e o `plan.md` identificam CB-001, CB-002 e CB-003 como comportamentos críticos.

**TDD**: TDD não é obrigatório; as verificações podem ser implementadas junto da funcionalidade, mas cada história deve ter evidência real antes do checkpoint.

## Format: `[ID] [P?] [Story] Description`

- **[P]**: pode executar em paralelo porque não há dependência nem conflito de arquivos;
- **[Story]**: identifica a user story relacionada;
- toda tarefa de implementação cita caminhos de arquivos concretos;
- toda tarefa de verificação indica o comando, a suíte, o cenário ou a evidência esperada.

## Path Conventions

O código ficará em `src/server/`, `src/client/` e `src/shared/`; migrações ficarão em `database/migrations/`; testes ficarão em `tests/unit/`, `tests/integration/`, `tests/component/` e `tests/e2e/`.

---

## Phase 1: Setup

**Purpose**: preparar o monorepo mínimo, as ferramentas e a execução local sem implementar comportamento de produto.

- [ ] T001 Criar `package.json` na raiz com scripts de desenvolvimento, build, lint, typecheck, testes unitários, integração, componente e E2E, registrando as versões escolhidas para React, runtime HTTP Node.js, PostgreSQL, validação runtime e runner de testes.
- [ ] T002 [P] Criar `tsconfig.json`, `tsconfig.server.json` e `tsconfig.client.json` com `strict: true`, saídas separadas e resolução de módulos coerente entre frontend, backend e contratos compartilhados.
- [ ] T003 [P] Criar `vite.config.ts`, `playwright.config.ts` e a configuração do runner de testes para executar componentes, unidades e integrações sem enviar e-mails reais.
- [ ] T004 [P] Criar `.env.example` e atualizar `.gitignore` com variáveis de banco, sessão, e-mail e ambiente sem incluir segredos reais ou códigos de autenticação.
- [ ] T005 Criar a estrutura inicial de `src/server/`, `src/client/`, `src/shared/`, `database/migrations/` e `tests/` conforme o plano, com pontos de entrada mínimos em `src/server/index.ts` e `src/client/main.tsx`.

**Checkpoint**: o projeto instala dependências, compila com TypeScript estrito e expõe comandos de desenvolvimento e teste sem segredos versionados.

---

## Phase 2: Foundational

**Purpose**: estabelecer persistência, contratos compartilhados e fronteiras confiáveis que bloqueiam todas as histórias.

- [ ] T006 Criar `src/shared/contracts/http.ts` com tipos explícitos para tarefas, status `pending`/`completed`, respostas de autenticação e formato estruturado de erro `{ error: { code, message } }`.
- [ ] T007 Criar `database/migrations/001_initial_schema.sql` com as tabelas `users`, `tasks`, `email_challenges` e `sessions`, chaves estrangeiras, unicidade de e-mail normalizado, índices de consulta, `status` restrito a `pending`/`completed` e campos de data obrigatórios conforme `data-model.md`.
- [ ] T008 Criar `src/server/infrastructure/postgres.ts` com um pool PostgreSQL por processo, queries parametrizadas, configuração por ambiente e encerramento limpo do pool.
- [ ] T009 Criar `src/server/http/errors.ts` e `src/server/http/server.ts` para converter erros conhecidos e inesperados no contrato estruturado sem expor SQL, stack trace, tokens ou detalhes de infraestrutura.
- [ ] T010 [P] Criar `src/server/http/validation.ts` para validar e-mail, código, título e status nas bordas HTTP, rejeitando título sem conteúdo e valores fora dos enums definidos no contrato.
- [ ] T011 [P] Criar `src/client/api/http-client.ts` e `src/client/api/types.ts` para chamadas HTTP com cookie de sessão, parsing dos contratos compartilhados, estados de carregamento e propagação explícita de erros de persistência.
- [ ] T012 [P] Criar `tests/integration/helpers/postgres.ts` com provisionamento determinístico do PostgreSQL de teste, aplicação das migrações e limpeza isolada entre testes.
- [ ] T013 [P] Criar `tests/component/test-setup.ts` e `tests/e2e/helpers/mailbox.ts` com fixtures determinísticas para UI e captura de códigos sem registrar o segredo em logs.

**Checkpoint**: banco, contratos, validação, transporte de erros e fixtures compartilhados estão disponíveis para as histórias sem depender de infraestrutura externa de e-mail.

---

## Phase 3: User Story 1 - Criar e consultar tarefas (Priority: P1)

**Goal**: permitir solicitar e validar código de e-mail, criar o usuário novo somente após validação, iniciar sessão e criar/consultar tarefas persistidas.

**Independent Test**: solicitar código para e-mail novo, validar o código, criar uma tarefa, sair, autenticar novamente e confirmar que a tarefa e seu estado pendente continuam disponíveis; repetir a consulta com um usuário diferente para confirmar isolamento.

### Verification for User Story 1

- [ ] T014 [P] [US1] Criar `tests/unit/auth-code.test.ts` para provar geração opaca, expiração, limite de tentativas, invalidação do código anterior, uso único e ausência de criação de usuário para código inválido, expirado ou reutilizado.
- [ ] T015 [P] [US1] Criar `tests/integration/auth-flow.test.ts` para provar que `POST /auth/request` responde de forma equivalente para e-mails novos/existentes e que `POST /auth/verify` consome o desafio, cria usuário novo e sessão atomicamente.
- [ ] T016 [P] [US1] Criar `tests/integration/tasks-persistence.test.ts` para provar `GET /tasks` e `POST /tasks`, persistência no PostgreSQL, título sem conteúdo rejeitado e recuperação após nova sessão.
- [ ] T017 [P] [US1] Criar `tests/integration/tasks-authorization.test.ts` para provar que requisição sem sessão é rejeitada e que um usuário não consegue consultar ou alterar tarefas de outro usuário.
- [ ] T018 [P] [US1] Criar `tests/component/auth-and-task-list.test.tsx` para provar estados visíveis de solicitação/validação de código, carregamento, lista vazia, criação confirmada e falha de persistência.
- [ ] T019 [US1] Criar `tests/e2e/auth-and-create-task.spec.ts` para provar a jornada seletiva de primeiro acesso, criação de tarefa e recuperação em novo acesso usando a caixa de e-mail de teste.

### Implementation for User Story 1

- [ ] T020 [US1] Criar `src/server/domain/auth.ts` com tipos e regras de código temporário: código numérico opaco, digest protegido, expiração, uso único, limite de tentativas e normalização de e-mail, sem registrar o segredo.
- [ ] T021 [US1] Criar `src/server/domain/task.ts` com o tipo `Task`, status `pending`/`completed` e validação de título com conteúdo.
- [ ] T022 [US1] Criar `src/server/application/auth.ts` com os casos de uso de solicitar código e validar código, aplicando resposta anti-enumeração e mantendo em uma transação o consumo válido, criação do usuário novo e criação da sessão.
- [ ] T023 [US1] Criar `src/server/application/tasks.ts` com casos de uso para listar e criar tarefas, sempre derivando o usuário da sessão e persistindo título, status inicial `pending`, `createdAt` e `updatedAt`.
- [ ] T024 [US1] Criar `src/server/infrastructure/email.ts` com interface de envio e adaptador determinístico de desenvolvimento/teste, mantendo o provedor real fora dos testes automatizados e sem expor o código.
- [ ] T025 [US1] Criar `src/server/infrastructure/auth-repository.ts` com queries parametrizadas para usuários, desafios e sessões, incluindo consumo condicional por e-mail normalizado, expiração e `consumedAt`.
- [ ] T026 [US1] Criar `src/server/infrastructure/task-repository.ts` com queries parametrizadas para inserir e listar tarefas filtrando obrigatoriamente por `userId`.
- [ ] T027 [US1] Criar `src/server/http/auth-routes.ts` para implementar `POST /auth/request`, `POST /auth/verify` e `POST /auth/logout`, configurar cookie opaco `Secure`, `HttpOnly` e `SameSite` apropriado e traduzir erros conforme `contracts/http-api.md`.
- [ ] T028 [US1] Criar `src/server/http/task-routes.ts` para implementar `GET /tasks` e `POST /tasks`, exigindo sessão válida, validando entrada e retornando somente dados do usuário autenticado.
- [ ] T029 [US1] Criar `src/server/http/session-middleware.ts` para resolver a sessão pelo digest do cookie, rejeitar sessão revogada/expirada e disponibilizar somente o `userId` derivado server-side ao restante da requisição.
- [ ] T030 [US1] Criar `src/client/screens/AuthScreen.tsx` com fluxo exclusivo de código por e-mail, incluindo primeiro acesso, mensagens equivalentes para e-mails novos/existentes e estados de erro sem revelar enumeração.
- [ ] T031 [US1] Criar `src/client/screens/TaskListScreen.tsx`, `src/client/components/TaskForm.tsx` e `src/client/components/TaskList.tsx` para exibir lista, estado de cada tarefa, formulário de título e feedback de operação somente após confirmação do backend.
- [ ] T032 [US1] Integrar `src/client/main.tsx` com `src/client/api/http-client.ts`, `AuthScreen.tsx` e `TaskListScreen.tsx`, incluindo logout e carregamento inicial da lista.

### Completion for User Story 1

- [ ] T033 [US1] Executar `npm run test:unit`, `npm run test:integration`, `npm run test:component` e `npm run test:e2e -- tests/e2e/auth-and-create-task.spec.ts`; registrar os resultados reais e corrigir falhas antes do checkpoint.
- [ ] T034 [US1] Confirmar os cenários de aceitação 1–4 da US1 e os casos negativos de título vazio, código inválido/expirado/reutilizado, falha de persistência e ausência de sessão usando as suítes em `tests/`.

**Checkpoint**: US1 entrega o MVP funcional de autenticação por código, criação, consulta e persistência privada de tarefas.

---

## Phase 4: User Story 2 - Concluir e reabrir tarefas (Priority: P2)

**Goal**: permitir alternar uma tarefa existente entre pendente e concluída e preservar a transição em acessos futuros.

**Independent Test**: autenticar com uma tarefa pendente, concluí-la, reabri-la, confirmar os estados na interface e consultar novamente após nova sessão.

### Verification for User Story 2

- [ ] T035 [P] [US2] Estender `tests/unit/task-domain.test.ts` para provar somente as transições `pending -> completed` e `completed -> pending`, rejeitando status desconhecido e transições incompatíveis.
- [ ] T036 [P] [US2] Estender `tests/integration/tasks-persistence.test.ts` para provar atualização de status, `updatedAt` alterado, persistência após nova sessão e isolamento por proprietário.
- [ ] T037 [P] [US2] Estender `tests/component/task-list.test.tsx` para provar ações observáveis de concluir/reabrir, apresentação dos estados e falha sem atualização otimista permanente quando o backend falha.
- [ ] T038 [US2] Estender `tests/e2e/auth-and-create-task.spec.ts` ou criar `tests/e2e/task-status.spec.ts` para provar a jornada pendente → concluída → pendente e a preservação após novo acesso.

### Implementation for User Story 2

- [ ] T039 [US2] Estender `src/server/application/tasks.ts` com caso de uso de alteração de status que aceite somente `pending` ou `completed`, aplique as transições previstas e preserve autorização pelo usuário da sessão.
- [ ] T040 [US2] Estender `src/server/infrastructure/task-repository.ts` com atualização parametrizada de status e `updatedAt` condicionada ao `userId`, sem revelar se identificador pertence a outro usuário.
- [ ] T041 [US2] Estender `src/server/http/task-routes.ts` para implementar `PATCH /tasks/{taskId}/status`, validar `taskId` e status, exigir sessão e retornar a tarefa atualizada no formato do contrato.
- [ ] T042 [US2] Estender `src/client/api/http-client.ts`, `src/client/components/TaskList.tsx` e `src/client/screens/TaskListScreen.tsx` para concluir/reabrir tarefas, exibir o estado confirmado e informar falhas de persistência.

### Completion for User Story 2

- [ ] T043 [US2] Executar `npm run test:unit`, `npm run test:integration`, `npm run test:component` e o E2E de status aplicável; registrar resultados reais e confirmar CB-002.
- [ ] T044 [US2] Confirmar os cenários de aceitação 1–3 da US2, inclusive a preservação do estado após novo acesso e o caso de tarefa concluída que pode ser reaberta.

**Checkpoint**: US2 permite concluir e reabrir tarefas com regra de domínio, autorização, persistência e UI verificadas.

---

## Phase 5: User Story 3 - Excluir tarefas (Priority: P3)

**Goal**: excluir imediatamente somente a tarefa solicitada, removendo-a de consultas futuras sem confirmação adicional.

**Independent Test**: criar pelo menos duas tarefas, excluir uma, confirmar que apenas ela desaparece imediatamente e verificar sua ausência após novo acesso.

### Verification for User Story 3

- [ ] T045 [P] [US3] Estender `tests/integration/tasks-persistence.test.ts` para provar `DELETE /tasks/{taskId}`, remoção seletiva, ausência em consultas futuras, resposta `204` e proteção contra exclusão por outro usuário.
- [ ] T046 [P] [US3] Estender `tests/component/task-list.test.tsx` para provar exclusão imediata sem confirmação adicional, remoção apenas do item escolhido e mensagem de falha quando a persistência não é confirmada.
- [ ] T047 [US3] Criar `tests/e2e/task-deletion.spec.ts` para provar exclusão seletiva e ausência da tarefa removida após novo acesso.

### Implementation for User Story 3

- [ ] T048 [US3] Estender `src/server/application/tasks.ts` com caso de uso de exclusão imediata condicionado à sessão autenticada e ao proprietário da tarefa.
- [ ] T049 [US3] Estender `src/server/infrastructure/task-repository.ts` com `DELETE` parametrizado por `taskId` e `userId`, sem apagar tarefas não selecionadas nem expor dados de outro usuário.
- [ ] T050 [US3] Estender `src/server/http/task-routes.ts` para implementar `DELETE /tasks/{taskId}`, retornar `204` somente após persistência e traduzir ausência/acesso indevido sem revelar informações.
- [ ] T051 [US3] Estender `src/client/api/http-client.ts`, `src/client/components/TaskList.tsx` e `src/client/screens/TaskListScreen.tsx` para excluir sem confirmação adicional e atualizar a lista somente após resposta bem-sucedida.

### Completion for User Story 3

- [ ] T052 [US3] Executar `npm run test:integration`, `npm run test:component` e `npm run test:e2e -- tests/e2e/task-deletion.spec.ts`; registrar resultados reais e confirmar CB-003.
- [ ] T053 [US3] Confirmar os cenários de aceitação 1–2 da US3, o caso de falha de persistência e a inexistência da tarefa excluída em consultas posteriores.

**Checkpoint**: US3 permite exclusão seletiva, imediata, autorizada e persistente, sem regressão das histórias anteriores.

---

## Final Phase: Cross-Cutting Verification & Cleanup

**Purpose**: consolidar qualidade, segurança, documentação operacional e evidência final sem adicionar escopo de produto.

- [ ] T054 [P] Criar `tests/unit/authorization.test.ts` para cobrir decisões comuns de sessão válida, sessão revogada/expirada e proprietário divergente sem depender do browser.
- [ ] T055 [P] Criar `tests/integration/transaction-failures.test.ts` para provar rollback quando falhar uma etapa do primeiro acesso ou uma persistência de tarefa, sem apresentar sucesso parcial.
- [ ] T056 [P] Criar `tests/integration/migrations.test.ts` para aplicar `database/migrations/001_initial_schema.sql` em PostgreSQL real e verificar constraints, índices e tipos persistidos.
- [ ] T057 [P] Atualizar `quickstart.md` somente se os comandos reais de instalação, migração, execução e testes diferirem do guia em `specs/001-task-management/quickstart.md`.
- [ ] T058 Executar `npm run lint`, `npm run typecheck`, `npm run build` e todas as suítes automatizadas; registrar somente resultados efetivamente observados e desvios conhecidos em `specs/001-task-management/quickstart.md` quando necessário.
- [ ] T059 Revisar `src/server/`, `src/client/`, `src/shared/` e `database/migrations/` contra a Constitution, confirmando regras localizáveis, autorização server-side, SQL parametrizado, ausência de segredos e exclusão de recursos fora do escopo.
- [ ] T060 Executar verificação operacional controlada do adaptador de e-mail configurado sem registrar códigos, usando o procedimento documentado em `specs/001-task-management/quickstart.md`.

---

## Dependencies & Execution Order

### Phase Dependencies

- Phase 1 deve concluir a estrutura, scripts e tipagem antes das demais fases.
- Phase 2 depende de Phase 1 e bloqueia todas as user stories por fornecer banco, contratos, validação, sessão e fixtures.
- US1 depende de Phase 2 e é pré-requisito funcional para US2 e US3 porque ambas operam sobre a lista autenticada.
- US2 depende de US1 para ter criação, consulta e sessão funcionando; US3 também depende de US1 e pode ser desenvolvido em paralelo com US2 após a base de US1 estar estável.
- A fase final depende das três histórias, exceto tarefas de documentação que podem ser antecipadas quando o comando real já estiver disponível.

### Within Each User Story

As verificações de uma história podem ser criadas em paralelo quando usam arquivos diferentes; a implementação deve preservar a transação, a autorização server-side e os contratos compartilhados antes da integração HTTP/UI. O checkpoint só pode ser concluído após execução real das verificações indicadas.

### Parallel Opportunities

- Phase 1: T002–T004 podem executar em paralelo após T001 definir os scripts e dependências.
- Phase 2: T010–T013 podem executar em paralelo após T006–T009 definirem os contratos e fronteiras.
- US1: T014–T019 são verificações independentes por arquivo; T024–T026 podem executar em paralelo após os tipos de domínio e a infraestrutura PostgreSQL.
- US2: T035–T038 são verificações independentes por arquivo; T039–T040 e T042 podem avançar em arquivos distintos após T021–T023.
- US3: T045–T047 são verificações independentes por arquivo; T048–T049 e T051 podem avançar em arquivos distintos após US1.
- Fase final: T054–T057 podem executar em paralelo; T058–T060 dependem dos resultados e artefatos das tasks anteriores.

## Implementation Strategy

1. Entregar o MVP em US1: autenticação exclusivamente por código, primeiro acesso com criação pós-validação, sessão, criação e consulta persistida de tarefas.
2. Entregar US2 como incremento independente de estado, adicionando somente a transição pendente/concluída e sua evidência.
3. Entregar US3 como incremento independente de exclusão seletiva imediata e sua evidência.
4. Executar a fase final para confirmar transações, migrações, segurança, tipagem, build e documentação sem ampliar o escopo.

## Notes

- Nenhuma task adiciona colaboração, anexos, recorrência, notificações ou autenticação por link/magic link.
- As restrições de campos e enums devem ser preservadas literalmente conforme `data-model.md` e `contracts/http-api.md`.
- A versão do Node.js, framework HTTP, runner, validação runtime, migrações e provedor de e-mail devem ser escolhidos e registrados em `package.json`/configuração durante o setup, sem inventar uma decisão no documento de requisitos.
- Nenhum código de autenticação deve ser persistido em claro, enviado ao cliente fora do e-mail ou incluído em logs, métricas, analytics ou resultados de teste.
- Todos os itens permanecem desmarcados; somente execução real pode marcar uma task como concluída.
