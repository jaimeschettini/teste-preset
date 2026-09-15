# Changelog

Todas as mudanças relevantes deste preset serão registradas aqui.

## [0.1.0] - 2026-09-14

### Adicionado

- primeira versão de desenvolvimento do preset;
- Constitution de projeto baseada na Constitution de Engenharia do IA-dev Labs v1.0.0;
- templates de `spec`, `plan`, `tasks` e `checklist`;
- wrapper de `speckit.constitution` para preservar a base global;
- wrapper de `speckit.specify` para impedir invenção de regras de negócio;
- wrapper de `speckit.tasks` para tornar testes orientados a risco e remover TDD obrigatório.

### Ajustado durante validação da 0.1.0

- removido hard wrap da prosa Markdown dos artefatos do preset;
- adicionada regra para não inferir guardrails específicos do projeto a partir de uma descrição funcional;
- adicionada convenção explícita de uma linha física por parágrafo de prosa.
