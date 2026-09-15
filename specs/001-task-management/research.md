# Research: Gerenciamento de tarefas

## Autenticação exclusivamente por código enviado por e-mail

**Decision**: Usar somente código temporário enviado ao e-mail, com valor opaco e imprevisível, uso único e invalidação do desafio anterior ao emitir um novo. Não haverá autenticação por link/magic link nesta versão. Após a validação, criar sessão autenticada separada do desafio.

**Rationale**: O fluxo atende à decisão da spec para recuperar tarefas em diferentes dispositivos sem exigir senha ou abrir tokens em URLs. O código deve ser tratado como credencial temporária: armazenar apenas digest/HMAC, aplicar expiração no servidor, limitar tentativas e não registrar o segredo. A sessão deve ser um cookie opaco com `Secure`, `HttpOnly` e `SameSite` apropriado, invalidado no logout.

**Alternatives considered**: Link/magic link foi explicitamente excluído pelo produto por expor uma credencial em URL; senha e passkeys ampliam ou mudam o escopo; JWT no navegador dificulta revogação e não oferece vantagem necessária nesta aplicação.

## Privacidade e abuso no login

**Decision**: Solicitações para e-mails existentes e inexistentes devem retornar mensagem, status e formato equivalentes, sem confirmar a existência da conta. Reenvios devem ser limitados por conta e IP, sem bloqueio que permita negar serviço a uma conta conhecida. Falhas do provedor de e-mail devem permanecer genéricas para o solicitante e ser observáveis sem tokens.

**Rationale**: Evita enumeração de contas, spam e custos inesperados. A autorização continua separada da autenticação: o backend deve validar a sessão e o proprietário da tarefa em cada requisição.

**Alternatives considered**: Informar e-mail não cadastrado melhora feedback, mas revela inventário de contas; retries ilimitados aumentam entrega potencial, mas permitem abuso; autorização baseada em `user_id` enviado pelo cliente é vulnerável a acesso horizontal indevido.

## Arquitetura TypeScript e PostgreSQL

**Decision**: Adotar monólito modular pequeno com domínio/aplicação, HTTP e persistência separados. Usar TypeScript estrito, validação runtime nas bordas, SQL parametrizado e um pool PostgreSQL por processo. Usar interfaces apenas nas fronteiras necessárias.

**Rationale**: Mantém regras de tarefa testáveis sem banco ou browser, preserva legibilidade e evita microserviços, CQRS ou ORM obrigatório para um CRUD pequeno. PostgreSQL real é necessário nos testes de integração para provar constraints, tipos, SQL e transações.

**Alternatives considered**: MVC tradicional pode misturar SQL e regra; Clean/Hexagonal completa é excessiva; SQLite não prova comportamento específico do PostgreSQL; ORM ou query builder podem ser avaliados depois se a necessidade concreta surgir.

## Estratégia de testes

**Decision**: Cobrir regras e casos de uso com testes unitários; repositórios, autorização, migrações e transações com integração real; estados de interação com testes de componente; e poucos E2E para autenticação e jornada principal.

**Rationale**: Essa distribuição prova os comportamentos críticos da spec sem transformar E2E em substituto de toda a suíte. Testes devem verificar comportamento observável, com dados determinísticos e sem envio real de e-mails.

**Alternatives considered**: Cobertura percentual fixa não demonstra risco coberto; mocks totais não detectam SQL/schema incorretos; uma suíte E2E ampla seria lenta e frágil para este escopo.

## Fontes consultadas

- OWASP Forgot Password Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Forgot_Password_Cheat_Sheet.html
- OWASP Session Management Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Session_Management_Cheat_Sheet.html
- OWASP Authorization Cheat Sheet: https://cheatsheetseries.owasp.org/cheatsheets/Authorization_Cheat_Sheet.html
- NIST SP 800-63B-4: https://pages.nist.gov/800-63-4/sp800-63b.html
- TypeScript `strict`: https://www.typescriptlang.org/tsconfig/strict.html
- node-postgres pooling e queries: https://node-postgres.com/features/pooling
- PostgreSQL transactions: https://www.postgresql.org/docs/current/tutorial-transactions.html
- React Testing Library: https://testing-library.com/docs/react-testing-library/intro/
- Playwright: https://playwright.dev/docs/intro
