# Security and API Requirements Quality Checklist: Gerenciamento de tarefas

<!-- IA-dev Labs formatting: prosa em Markdown não usa hard wrap; mantenha cada parágrafo em uma única linha física e deixe a quebra visual para o editor. -->

**Purpose**: Revisar a qualidade dos requisitos de autenticação, autorização, contratos HTTP, persistência e falhas de segurança.

**Created**: 2026-09-15

**Feature**: [spec.md](../spec.md), [plan.md](../plan.md) e [contracts/http-api.md](../contracts/http-api.md)

**Note**: Este checklist é gerado pelo `$speckit-checklist` para revisar a QUALIDADE DOS REQUISITOS, não para confirmar que a implementação funciona.

**Review Ownership**: Marque `[x]` apenas quando o critério de qualidade do requisito tiver sido efetivamente revisado e satisfeito.

**Marker Semantics**: `[x]` significa “requisito revisado e adequado”. NÃO significa “implementação concluída”.

## Requirement Completeness

- [ ] CHK001 - A spec define autenticação para todos os recursos protegidos e deixa explícito que o acesso ocorre somente por código enviado por e-mail? [Completeness, Spec §FR-007]
- [ ] CHK002 - Os requisitos definem o que acontece quando o usuário solicita um código para um endereço existente e para um endereço inexistente? [Completeness, Spec §FR-007, Gap]
- [ ] CHK003 - Os requisitos cobrem a criação, validação, expiração, consumo único e invalidação de códigos de acesso? [Completeness, Plan §Decisões arquiteturais e limites da mudança, Data Model §EmailChallenge]
- [ ] CHK004 - Os requisitos definem autorização para leitura, alteração de estado e exclusão de tarefas pertencentes a outro usuário? [Completeness, Spec §FR-003–FR-006, Plan §Casos negativos de autorização]
- [ ] CHK005 - Os requisitos descrevem os estados de carregamento, lista vazia, validação e falha para as jornadas de interface previstas? [Completeness, Contract §Estados de interface]
- [ ] CHK006 - Os requisitos definem o comportamento esperado quando o envio de e-mail ou a persistência falha parcialmente? [Completeness, Spec §FR-008, Plan §Concorrência / falhas parciais]

## Requirement Clarity

- [ ] CHK007 - O termo “código enviado por e-mail” está definido com formato, canal e uso esperado suficientes para evitar interpretação como link ou senha permanente? [Clarity, Spec §Clarifications, Spec §FR-007]
- [ ] CHK008 - A expressão “título não contém conteúdo” está suficientemente clara para determinar quais entradas são inválidas? [Clarity, Spec §FR-002, Spec §Edge Cases]
- [ ] CHK009 - O requisito de recuperar tarefas em diferentes dispositivos identifica de forma inequívoca qual vínculo do usuário autoriza essa recuperação? [Clarity, Spec §FR-007, Data Model §User]
- [ ] CHK010 - A expressão “imediatamente após a solicitação” deixa claro o resultado esperado da exclusão e a ausência de confirmação adicional? [Clarity, Spec §FR-009]
- [ ] CHK011 - Os códigos de erro, mensagens seguras e condições de erro estão especificados com precisão suficiente para orientar respostas consistentes? [Clarity, Contract §Convenção de erro]

## Requirement Consistency

- [ ] CHK012 - A decisão de excluir autenticação por link/magic link é consistente entre Clarifications, FR-007, plano, pesquisa, modelo de dados e contrato? [Consistency, Conflict]
- [ ] CHK013 - A exigência de exclusão imediata é consistente entre FR-009, cenários de aceitação, comportamento crítico CB-003 e SC-003? [Consistency, Spec §FR-009, Spec §CB-003, Spec §SC-003]
- [ ] CHK014 - Os requisitos de persistência entre acessos são consistentes com a autorização por usuário e não permitem que um identificador do cliente defina propriedade? [Consistency, Spec §FR-007, Plan §Casos negativos de autorização, Contract §Tarefas autenticadas]
- [ ] CHK015 - Os estados `pending` e `completed` são usados de forma consistente na spec, no modelo de dados e no contrato HTTP? [Consistency, Spec §FR-003–FR-005, Data Model §Task, Contract §PATCH /tasks/{taskId}/status]

## Acceptance Criteria Quality

- [ ] CHK016 - Os critérios de sucesso medem separadamente preservação, transição de estado, exclusão seletiva e compreensão do fluxo principal? [Acceptance Criteria, Spec §SC-001–SC-004]
- [ ] CHK017 - O uso de “100%” nos critérios de sucesso está associado a uma população ou avaliação funcional claramente definida? [Measurability, Spec §SC-001–SC-003]
- [ ] CHK018 - Cada comportamento crítico possui evidência observável alinhada ao risco descrito, sem depender de detalhes de implementação? [Traceability, Spec §CB-001–CB-003]
- [ ] CHK019 - Os cenários de aceitação permitem distinguir sucesso persistido de uma alteração apenas apresentada temporariamente pela interface? [Clarity, Spec §Edge Cases, Spec §FR-008]

## Scenario Coverage

- [ ] CHK020 - As jornadas primárias de criar, consultar, concluir, reabrir, excluir e recuperar tarefas em outro dispositivo estão todas representadas? [Coverage, Spec §User Scenarios & Testing]
- [ ] CHK021 - Os requisitos cobrem o fluxo alternativo de solicitar um novo código quando o anterior está expirado, consumido ou inválido? [Coverage, Plan §Riscos e recuperação, Gap]
- [ ] CHK022 - Os requisitos definem o comportamento de sessão expirada, revogada ou ausente para recursos protegidos? [Coverage, Data Model §Session, Contract §Tarefas autenticadas, Gap]
- [ ] CHK023 - Os requisitos cobrem o cenário de duas tarefas do mesmo usuário para demonstrar exclusão seletiva sem ambiguidade? [Coverage, Spec §CB-003, Spec §SC-003]

## Edge Case Coverage

- [ ] CHK024 - As condições de entrada inválida, código incorreto, código expirado e código reutilizado estão explicitamente diferenciadas ou agrupadas com comportamento seguro? [Edge Case, Contract §POST /auth/verify]
- [ ] CHK025 - Os requisitos definem limites ou resposta para tentativas repetidas e reenvios de código sem criar uma regra de negócio implícita? [Completeness, Research §Privacidade e abuso no login, Gap]
- [ ] CHK026 - Os requisitos definem como tratar concorrência entre duas alterações de estado ou entre exclusão e alteração da mesma tarefa? [Coverage, Plan §Concorrência / falhas parciais, Gap]
- [ ] CHK027 - O cenário de lista sem tarefas está definido como estado de produto, e não apenas como ausência genérica de dados? [Clarity, Contract §Estados de interface, Gap]

## Non-Functional Requirements

- [ ] CHK028 - Os requisitos de segurança deixam claro que códigos, sessões e credenciais não devem ser expostos em logs, mensagens ou dados observáveis ao cliente? [Completeness, Plan §Constraints, Data Model §EmailChallenge e §Session]
- [ ] CHK029 - Os requisitos especificam proteção suficiente contra enumeração de contas sem contradizer a necessidade de feedback ao usuário? [Consistency, Research §Privacidade e abuso no login, Contract §POST /auth/request]
- [ ] CHK030 - Os requisitos de privacidade e autorização definem explicitamente que cada tarefa só pode ser operada pelo usuário proprietário? [Security, Spec §FR-007, Plan §Casos negativos de autorização]
- [ ] CHK031 - Os requisitos de acessibilidade e localização da interface são definidos ou explicitamente reconhecidos como lacunas fora do escopo atual? [Completeness, Gap]
- [ ] CHK032 - A ausência de metas de desempenho e escala está explicitamente delimitada para evitar que termos como “simples” sejam interpretados como garantia operacional? [Clarity, Plan §Performance Goals e §Scale/Scope]

## Dependencies & Assumptions

- [ ] CHK033 - A dependência do provedor de e-mail está documentada com seus modos de falha e sem pressupor disponibilidade ilimitada? [Dependency, Plan §Contratos externos, Plan §Riscos e recuperação]
- [ ] CHK034 - As assumptions distinguem claramente o que é decisão de produto do que é dependência operacional ou escolha futura de implementação? [Assumption, Spec §Assumptions, Plan §Technical Context]
- [ ] CHK035 - A especificação identifica quais partes do fluxo dependem de um e-mail realmente entregue e quais podem ser validadas sem serviço externo? [Dependency, Research §Privacidade e abuso no login, Quickstart §Falhas e segurança]
- [ ] CHK036 - A estratégia de recuperação para falhas de persistência, consumo atômico de código e migrações está documentada sem prometer rollback onde ele não foi definido? [Completeness, Constitution §V, Plan §Riscos e recuperação]

## Ambiguities & Conflicts

- [ ] CHK037 - Está inequívoco se um código é numérico, qual sua validade e qual limite de tentativas se aplica, ou essas decisões continuam pendentes para o plano? [Ambiguity, Spec §FR-007, Plan §Constraints, Research §Expiração]
- [ ] CHK038 - O contrato diferencia ausência de sessão, tarefa inexistente e tarefa pertencente a outro usuário sem criar vazamento de informação? [Clarity, Contract §Tarefas autenticadas, Plan §Casos negativos de autorização]
- [ ] CHK039 - Os requisitos deixam claro se a exclusão é irreversível nesta versão e se não existe fluxo de restauração? [Ambiguity, Spec §FR-006 e §FR-009, Data Model §Task]
- [ ] CHK040 - Existe uma referência rastreável entre cada requisito funcional, seus cenários de aceitação e pelo menos um critério de sucesso ou comportamento crítico? [Traceability, Spec §FR-001–FR-009, Spec §CB-001–CB-003, Spec §SC-001–SC-004]

## Notes

- Todos os 40 itens são perguntas de qualidade dos requisitos; nenhum item confirma comportamento da implementação.
- 40/40 itens possuem referência à spec, ao plano, aos artefatos de design ou a um marcador explícito de lacuna/ambiguidade/conflito.
- Os itens foram gerados para revisão de requisitos, não para substituir testes unitários, de integração, de componente ou E2E.
- Itens novos permanecem desmarcados; `[x]` só deve ser aplicado por um revisor após análise real.
