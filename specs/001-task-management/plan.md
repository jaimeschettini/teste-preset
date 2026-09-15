# Implementation Plan: Gerenciamento de tarefas

<!-- IA-dev Labs formatting: prosa em Markdown não usa hard wrap; mantenha cada parágrafo em uma única linha física e deixe a quebra visual para o editor. -->

**Branch**: `[001-task-management]` | **Date**: 2026-09-15 | **Spec**: [spec.md](./spec.md)

**Input**: Feature specification from `/specs/001-task-management/spec.md`

## Summary

Construir uma aplicação web monolítica e modular para gerenciamento privado de tarefas, com React e TypeScript no frontend, Node.js e TypeScript no backend e PostgreSQL como persistência. O backend concentrará autenticação, autorização, regras de tarefa e acesso ao banco; o frontend consumirá contratos HTTP explícitos. O acesso entre dispositivos usará somente código de uso único enviado por e-mail, seguido de sessão autenticada por cookie seguro. A solução evita microserviços, ORM obrigatório e camadas cerimoniais.

## Technical Context

**Language/Version**: TypeScript estrito no frontend e backend; versão do Node.js a fixar no setup do projeto

**Primary Dependencies**: React, runtime HTTP Node.js, cliente PostgreSQL, biblioteca de validação de entradas e ferramenta de testes a escolher na implementação

**Storage**: PostgreSQL com migrações versionadas; tarefas pertencem a um usuário identificado e desafios podem existir para e-mails ainda não cadastrados até sua validação

**Testing**: testes unitários de domínio/casos de uso, integração com PostgreSQL real, testes de componente/UI e poucos testes end-to-end dos fluxos críticos

**Target Platform**: navegadores modernos e servidor Node.js

**Project Type**: aplicação web monolítica com frontend e backend TypeScript

**Performance Goals**: N/A; a spec não define metas quantitativas de latência ou volume

**Constraints**: tipagem estrita; SQL parametrizado; autenticação e autorização em fronteira confiável; códigos numéricos de e-mail opacos, temporários e de uso único; cookie de sessão seguro; mensagens de login sem enumeração de contas; escopo de tarefa filtrado pelo usuário autenticado; colaboração, anexos, recorrência, notificações e autenticação por link/magic link fora do escopo

**Scale/Scope**: uma pessoa gerenciando suas próprias tarefas; volume e concorrência não foram especificados e devem permanecer proporcionais a uma primeira versão simples

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

- **Regras de domínio localizáveis**: PASS — estados e transições de Tarefa ficarão em tipos e casos de uso do backend, separados de HTTP e PostgreSQL.
- **Fronteiras de confiança e segurança**: PASS — sessão, autenticação por e-mail e autorização serão validadas no backend; cada operação filtrará pelo usuário autenticado.
- **Legibilidade e simplicidade arquitetural**: PASS — monólito modular pequeno, sem microserviços, CQRS ou arquitetura hexagonal completa.
- **Contratos e tipagem**: PASS — entradas, respostas, estados e erros terão contratos TypeScript explícitos e validação nas bordas.
- **Consistência / atomicidade / idempotência**: PASS — consumo do código e criação do usuário no primeiro acesso serão atômicos; alterações de tarefa serão persistidas como operações coerentes; retries de envio serão limitados e observáveis.
- **Verificação automatizada**: PASS — estratégia cobre os comportamentos CB-001, CB-002 e CB-003 com unidade, integração, UI e E2E seletivo.
- **Responsabilidade e evidência dos agentes**: PASS — quickstart e tarefas deverão registrar comandos e resultados reais, sem declarar validações não executadas.
- **Guardrails específicos do projeto**: PASS — nenhum guardrail adicional foi definido; a baseline de segurança e testes será preservada.

## Decisões arquiteturais e limites da mudança

**Responsabilidades alteradas**: autenticação exclusivamente por código, sessão, autorização por proprietário, casos de uso de tarefa, persistência PostgreSQL, rotas HTTP e telas React de lista/criação/estado/exclusão.

**Responsabilidades preservadas**: colaboração, anexos, recorrência e notificações não serão modelados nem implementados nesta versão.

**Novas abstrações**: interfaces pequenas para repositórios e envio de e-mail somente nas fronteiras necessárias para testar casos de uso e substituir infraestrutura; nenhum framework ou padrão arquitetural adicional é obrigatório.

**Contratos externos**: contrato HTTP documentado em `contracts/http-api.md`; provedor de e-mail é uma dependência operacional, sem contrato específico escolhido nesta fase.

**Concorrência / falhas parciais**: consumo de desafio de autenticação e criação do usuário no primeiro acesso devem ocorrer atomicamente; atualizações de tarefa devem verificar o proprietário na mesma operação; falha de persistência não pode ser apresentada como sucesso; falha de e-mail mantém resposta genérica ao solicitante e gera sinal operacional sem registrar código.

## Estratégia de testes e verificação *(mandatory)*

| Comportamento crítico | Nível de teste | Por que este nível prova a propriedade | Evidência adicional |
|-----------------------|----------------|----------------------------------------|---------------------|
| CB-001 — preservação entre acessos | Integração + E2E seletivo | Integração prova mapeamento e persistência reais no PostgreSQL; E2E prova primeiro acesso, criação pós-validação e recuperação autenticada | PostgreSQL real, cenário de novo acesso |
| CB-002 — transição de estado | Unidade + integração + componente/UI | Unidade prova a regra de transição; integração prova gravação; UI prova apresentação e ação observáveis | Cenários pendente → concluída → pendente |
| CB-003 — exclusão da tarefa solicitada | Integração + componente/UI + E2E seletivo | Integração prova filtro por proprietário e remoção correta; UI/E2E provam que somente a tarefa escolhida desaparece | Cenário com mais de uma tarefa |

**Propriedades críticas sem automação**: nenhuma identificada; a entregabilidade real do e-mail deve ser verificada operacionalmente sem incluir tokens em logs.

**Casos negativos de autorização**: requisição sem sessão e usuário autenticado tentando acessar identificador de tarefa pertencente a outro usuário devem receber resposta de acesso negado ou inexistência indistinguível, sem revelar dados.

**Chamadas reais a terceiros**: testes não devem enviar e-mails reais; usar adaptador determinístico e teste operacional separado do provedor configurado.

## Riscos e recuperação

| Risco / modo de falha | Impacto | Mitigação / recuperação | Como verificar |
|-----------------------|---------|-------------------------|----------------|
| Reutilização ou vazamento de código de e-mail | Acesso indevido à conta | Digest/HMAC no banco, expiração, uso único, invalidação do código anterior, cookie seguro e não exposição do código | Testes de replay, expiração e inspeção de logs |
| Enumeração de contas ou abuso de reenvio | Exposição de usuários, spam e custo | Resposta uniforme, limites por conta/IP e retries controlados | Testes de respostas equivalentes e rate limit |
| Tarefa de outro usuário acessível por identificador | Violação de privacidade | Autorização server-side e filtro por proprietário em toda consulta/mutação | Teste de acesso cruzado |
| Falha parcial de persistência | Interface diverge do estado real | Confirmar sucesso somente após persistência; rollback em transações | Teste de erro de banco e reconsulta |
| Complexidade excessiva para o escopo | Custo e manutenção desnecessários | Monólito modular, SQL direto e abstrações apenas nas fronteiras | Revisão estrutural contra este plano |

## Project Structure

### Documentation (this feature)

```text
specs/001-task-management/
├── plan.md
├── research.md
├── data-model.md
├── quickstart.md
├── contracts/
│   └── http-api.md
└── tasks.md
```

### Source Code (repository root)

```text
src/
├── server/
│   ├── domain/
│   ├── application/
│   ├── infrastructure/
│   └── http/
├── client/
│   ├── components/
│   ├── screens/
│   └── api/
└── shared/
    └── contracts/
database/
└── migrations/
tests/
├── unit/
├── integration/
├── component/
└── e2e/
```

**Structure Decision**: O repositório está sem código-fonte, então esta estrutura inicial explicita as fronteiras mínimas entre domínio/aplicação, infraestrutura/HTTP e UI. O ponto de composição conectará implementações concretas sem criar módulos de colaboração ou recursos fora do escopo.

## Complexity Tracking

Nenhuma violação constitucional ou complexidade material sem justificativa foi identificada.
