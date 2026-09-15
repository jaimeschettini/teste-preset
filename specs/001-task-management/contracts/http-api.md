# Contrato HTTP: Gerenciamento de tarefas

Este contrato descreve as interfaces observáveis entre o cliente React e o backend Node.js. Os nomes de campos são estáveis para a feature; o framework HTTP e o formato final de documentação automática permanecem decisões de implementação.

## Convenção de erro

Respostas de erro usam formato estruturado:

```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Mensagem segura para o usuário"
  }
}
```

Erros inesperados retornam mensagem genérica e não expõem SQL, stack trace, tokens ou detalhes de infraestrutura.

## Autenticação

### `POST /auth/request`

Solicita código ou link de acesso para um endereço de e-mail.

Request:

```json
{ "email": "user@example.com" }
```

Response `202` para e-mails existentes ou inexistentes, com mensagem e formato equivalentes:

```json
{ "message": "Se houver uma conta para esse endereço, enviaremos as instruções." }
```

O backend deve aplicar limites de envio e não revelar se a conta existe.

### `POST /auth/verify`

Valida código ou token de link de uso único e cria sessão autenticada.

Request:

```json
{ "email": "user@example.com", "challenge": "opaque-value" }
```

Response `204` em sucesso e cookie de sessão seguro. Desafios inválidos, expirados, consumidos ou excedidos retornam erro estruturado sem revelar detalhes que permitam replay ou enumeração.

### `POST /auth/logout`

Revoga a sessão atual e limpa o cookie. Response `204`.

## Tarefas autenticadas

Todas as rotas abaixo exigem sessão válida. O usuário autenticado é derivado da sessão, nunca de um `userId` enviado pelo cliente.

### `GET /tasks`

Retorna as tarefas do usuário autenticado.

Response `200`:

```json
{
  "tasks": [
    {
      "id": "task-id",
      "title": "Preparar apresentação",
      "status": "pending",
      "createdAt": "2026-09-15T12:00:00.000Z",
      "updatedAt": "2026-09-15T12:00:00.000Z"
    }
  ]
}
```

### `POST /tasks`

Cria tarefa pendente.

Request:

```json
{ "title": "Preparar apresentação" }
```

Resposta `201` com a tarefa criada. Título sem conteúdo retorna `400` e não cria registro.

### `PATCH /tasks/{taskId}/status`

Altera o estado de uma tarefa existente.

Request:

```json
{ "status": "completed" }
```

`status` aceita somente `pending` ou `completed`. A operação deve preservar a autorização por proprietário e retornar `200` com a tarefa atualizada.

### `DELETE /tasks/{taskId}`

Exclui imediatamente a tarefa pertencente ao usuário autenticado. Response `204`. Uma tarefa de outro usuário não deve ser removida nem ter seus dados revelados.

## Estados de interface

O cliente deve representar estados de carregamento, sucesso, lista vazia, validação de título e falha de persistência sem tratar uma operação não confirmada pelo backend como concluída.
