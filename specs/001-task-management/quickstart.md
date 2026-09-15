# Quickstart de validação

Este guia valida a jornada principal conforme [data-model.md](./data-model.md) e [contracts/http-api.md](./contracts/http-api.md).

## Pré-requisitos

- Node.js na versão definida pelo projeto durante a implementação.
- PostgreSQL disponível localmente ou em ambiente efêmero de testes.
- Configuração de ambiente para conexão PostgreSQL e envio de e-mail de teste.
- Dependências instaladas pelo gerenciador escolhido no setup.

## Preparação

1. Instalar dependências do frontend e backend.
2. Configurar as variáveis de ambiente sem versionar segredos.
3. Criar o banco de desenvolvimento.
4. Aplicar as migrações versionadas.
5. Iniciar backend e frontend pelos scripts do projeto.

## Cenários de validação

### Persistência e acesso entre dispositivos

1. Solicitar acesso para um e-mail de teste e obter o código/link pelo adaptador de e-mail controlado.
2. Validar o desafio e criar uma tarefa com título não vazio.
3. Encerrar a sessão e iniciar novo acesso usando o mesmo e-mail em outro contexto de navegador/dispositivo.
4. Verificar que a tarefa permanece listada com o estado `pending`.

### Concluir e reabrir

1. Marcar a tarefa como concluída.
2. Verificar a apresentação `completed`.
3. Reabrir a tarefa.
4. Verificar a apresentação `pending` após nova consulta.

### Excluir somente a tarefa escolhida

1. Criar duas tarefas.
2. Excluir uma delas.
3. Verificar que a tarefa escolhida desapareceu imediatamente.
4. Atualizar a lista e verificar que a outra tarefa permanece.

### Falhas e segurança

1. Submeter título vazio e verificar validação sem criação.
2. Reutilizar desafio de e-mail consumido e verificar rejeição.
3. Usar desafio expirado e verificar rejeição.
4. Tentar acessar tarefa de outro usuário e verificar ausência de autorização sem vazamento de dados.
5. Simular falha de persistência e verificar mensagem de erro sem falso sucesso.

## Comandos de verificação

Executar os scripts definidos no projeto para:

- verificação de tipos estrita;
- lint e formatação;
- testes unitários;
- testes de integração com PostgreSQL real;
- testes de componente/UI;
- suíte E2E seletiva.

Os resultados observados devem ser registrados pelo agente de implementação; este documento não declara resultados antes da execução.
