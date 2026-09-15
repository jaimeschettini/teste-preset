# Validation report - IA-dev Labs preset v0.1.0

Validações concluídas durante o teste local:

- `preset.yml` parseia como YAML válido;
- schema version é `1.0`;
- preset id é `ia-dev-labs`;
- todos os arquivos referenciados pelo manifest existem;
- os três comandos wrapped contêm `{CORE_TEMPLATE}`;
- instalação local com `specify preset add --dev` concluída sem erro;
- `specify preset list` reconheceu o preset `ia-dev-labs` v0.1.0 como habilitado;
- os cinco templates resolvem para a camada `ia-dev-labs` v0.1.0;
- `speckit.constitution`, `speckit.specify` e `speckit.tasks` resolvem como composição `core` + `wrap ia-dev-labs`;
- o primeiro teste real de `/speckit-constitution` gerou uma Constitution autocontida e registrou `IA-dev Labs v1.0.0` como base;
- a validação revelou e motivou dois refinamentos ainda dentro da v0.1.0: impedir inferência de guardrails específicos e remover hard wrap da prosa Markdown.

Ainda falta validar:

- nova execução de `/speckit-constitution` após os refinamentos;
- geração de `spec.md` sem invenção de regras de negócio;
- geração de `plan.md` com estratégia de testes explícita;
- geração de `tasks.md` com verificação adequada ao risco e sem TDD obrigatório;
- comportamento do `checklist-template`;
- compatibilidade end-to-end com o agente/integration escolhido para o projeto piloto.
