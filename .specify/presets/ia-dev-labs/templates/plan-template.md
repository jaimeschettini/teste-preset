# Implementation Plan: [FEATURE]

<!-- IA-dev Labs formatting: prosa em Markdown não usa hard wrap; mantenha cada parágrafo em uma única linha física e deixe a quebra visual para o editor. -->

**Branch**: `[###-feature-name]` | **Date**: [DATE] | **Spec**: [link]

**Input**: Feature specification from `/specs/[###-feature-name]/spec.md`

**Note**: Este template é preenchido pelo `$speckit-plan`. Os nomes que funcionam como contrato do GitHub Spec Kit são preservados em inglês; o conteúdo humano deve ser escrito em português brasileiro.

## Summary

[Resumir requisito principal, abordagem técnica e limites relevantes sem repetir toda a spec]

## Technical Context

**Language/Version**: [stack real ou NEEDS CLARIFICATION]

**Primary Dependencies**: [dependências necessárias ou N/A]

**Storage**: [persistência aplicável ou N/A]

**Testing**: [ferramentas e mecanismos existentes/previstos]

**Target Platform**: [plataforma real]

**Project Type**: [tipo real de projeto]

**Performance Goals**: [metas sustentadas pela spec ou N/A; não inventar números]

**Constraints**: [restrições técnicas, operacionais, segurança, compatibilidade]

**Scale/Scope**: [escala conhecida ou NEEDS CLARIFICATION quando material]

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

<!--
Avalie a Constitution real do projeto. Para cada princípio relevante:
  - PASS: como o plano preserva a garantia;
  - N/A: por que não se aplica;
  - VIOLATION: somente com justificativa explícita e decisão humana antes da implementação.

Não trate a existência do documento como aprovação automática.
-->

- **Regras de domínio localizáveis**: [PASS / N/A / VIOLATION + evidência]
- **Fronteiras de confiança e segurança**: [PASS / N/A / VIOLATION + evidência]
- **Legibilidade e simplicidade arquitetural**: [PASS / N/A / VIOLATION + evidência]
- **Contratos e tipagem**: [PASS / N/A / VIOLATION + evidência]
- **Consistência / atomicidade / idempotência**: [PASS / N/A / VIOLATION + evidência]
- **Verificação automatizada**: [PASS / N/A / VIOLATION + evidência]
- **Responsabilidade e evidência dos agentes**: [PASS / N/A / VIOLATION + evidência]
- **Guardrails específicos do projeto**: [PASS / N/A / VIOLATION + evidência]

## Decisões arquiteturais e limites da mudança

<!--
Descrever a solução proporcional ao problema atual. Não adicionar layers, repositories, services, adapters, wrappers, factories ou outras abstrações apenas por preferência arquitetural. Toda complexidade nova deve resolver uma necessidade concreta.
-->

**Responsabilidades alteradas**: [módulos/casos de uso/fronteiras que mudam]

**Responsabilidades preservadas**: [áreas que explicitamente não serão redesenhadas]

**Novas abstrações**: [nenhuma, ou listar cada uma com o problema concreto que resolve]

**Contratos externos**: [fontes autoritativas, versões e riscos; N/A quando não houver]

**Concorrência / falhas parciais**: [estratégia quando aplicável]

## Estratégia de testes e verificação *(mandatory)*

<!--
Para cada comportamento crítico da spec, selecione o nível mais simples que consegue provar a propriedade. Não escolher por hábito, cobertura ou preferência de ferramenta.

Níveis usuais:
  - domínio/unidade;
  - integração;
  - componente/UI;
  - end-to-end;
  - contrato/integração externa.

TDD não é obrigatório.
-->

| Comportamento crítico | Nível de teste | Por que este nível prova a propriedade | Evidência adicional |
|-----------------------|----------------|----------------------------------------|---------------------|
| CB-001 | [nível] | [justificativa] | [browser/manual/contrato/N/A] |

**Propriedades críticas sem automação**: [nenhuma, ou registrar limitação, risco e verificação alternativa]

**Casos negativos de autorização**: [quando aplicável]

**Chamadas reais a terceiros**: [estratégia controlada ou N/A]

## Riscos e recuperação

| Risco / modo de falha | Impacto | Mitigação / recuperação | Como verificar |
|-----------------------|---------|-------------------------|----------------|
| [risco] | [impacto] | [estratégia] | [evidência] |

## Project Structure

### Documentation (this feature)

```text
specs/[###-feature]/
├── plan.md
├── research.md
├── data-model.md
├── quickstart.md
├── contracts/
└── tasks.md
```

### Source Code (repository root)

<!--
Substitua pelo layout REAL afetado pela feature. Não use este espaço para propor uma nova arquitetura genérica. Liste caminhos concretos existentes e novos caminhos apenas quando o plano justificar sua criação.
-->

```text
[caminhos reais afetados]
```

**Structure Decision**: [Explicar por que a mudança cabe nessa estrutura e quais limites foram preservados]

## Complexity Tracking

> **Fill ONLY if Constitution Check has violations or the plan introduces complexity material that
> requires explicit justification.**

| Violation / Complexity | Why Needed | Simpler Alternative Rejected Because | Approval |
|------------------------|------------|-------------------------------------|----------|
| [item] | [necessidade atual] | [alternativa considerada] | [decisão humana] |
