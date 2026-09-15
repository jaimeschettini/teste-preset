# Modelo de dados: Gerenciamento de tarefas

## User

Representa a pessoa que possui uma lista privada de tarefas e é identificada pelo endereço de e-mail usado no fluxo de acesso.

| Campo | Tipo lógico | Obrigatório | Regras |
|------|-------------|-------------|--------|
| id | identificador | Sim | Identifica exclusivamente o usuário |
| email | texto | Sim | Normalizado para identificação; não é exposto como autorização pelo cliente |
| createdAt | data/hora | Sim | Momento de criação do usuário |

## Task

Representa um item que o usuário acompanha.

| Campo | Tipo lógico | Obrigatório | Regras |
|------|-------------|-------------|--------|
| id | identificador | Sim | Identifica exclusivamente a tarefa |
| userId | identificador | Sim | Referencia o proprietário; toda consulta e mutação deve restringir-se a este vínculo |
| title | texto | Sim | Deve conter conteúdo após validação de entrada |
| status | enum | Sim | `pending` ou `completed`; inicia em `pending` |
| createdAt | data/hora | Sim | Momento de criação |
| updatedAt | data/hora | Sim | Atualizado a cada mudança persistida |

### Transições de estado

```text
pending ── concluir ──> completed
completed ── reabrir ──> pending
```

Não há transição de exclusão reversível prevista; a tarefa removida não deve aparecer em consultas futuras.

## EmailChallenge

Representa um desafio temporário para provar controle do e-mail e iniciar uma sessão.

| Campo | Tipo lógico | Obrigatório | Regras |
|------|-------------|-------------|--------|
| id | identificador | Sim | Identifica o desafio |
| userId | identificador | Sim | Usuário ao qual o desafio pertence |
| secretDigest | bytes/texto protegido | Sim | Nunca armazenar o código ou token em claro |
| kind | enum | Sim | `code` ou `link` |
| expiresAt | data/hora | Sim | Após expirar, o desafio falha |
| consumedAt | data/hora anulável | Não | Preenchido atomicamente no consumo bem-sucedido |
| attemptCount | inteiro | Sim | Limitado para reduzir tentativas automatizadas |
| createdAt | data/hora | Sim | Momento de emissão |

Somente o desafio mais recente do usuário deve permanecer válido. O segredo não deve aparecer em logs, métricas, mensagens ou analytics.

## Session

Representa a sessão autenticada criada após validação do desafio.

| Campo | Tipo lógico | Obrigatório | Regras |
|------|-------------|-------------|--------|
| id | identificador | Sim | Identifica a sessão no servidor |
| userId | identificador | Sim | Usuário autenticado |
| secretDigest | bytes/texto protegido | Sim | Cookie contém apenas valor opaco; digest fica no servidor |
| expiresAt | data/hora | Sim | Limite absoluto da sessão |
| lastSeenAt | data/hora | Sim | Permite política de inatividade |
| revokedAt | data/hora anulável | Não | Preenchido no logout ou revogação |
| createdAt | data/hora | Sim | Momento de autenticação |

## Relações e invariantes

- Um `User` possui zero ou muitas `Task`, `EmailChallenge` e `Session`.
- Uma `Task` pertence a exatamente um `User`.
- Um `EmailChallenge` consumido, expirado ou substituído não pode autenticar novamente.
- Uma `Session` revogada ou expirada não pode autorizar requisições.
- Identificadores recebidos em rotas nunca substituem a verificação server-side de `userId`.
