# Feature Specification: [FEATURE NAME]

<!-- IA-dev Labs formatting: prosa em Markdown não usa hard wrap; mantenha cada parágrafo em uma única linha física e deixe a quebra visual para o editor. -->

**Feature Branch**: `[###-feature-name]`

**Created**: [DATE]

**Status**: Draft

**Input**: User description: "$ARGUMENTS"

## Contexto e escopo

<!--
Resuma o problema, o resultado desejado, o que está dentro do escopo e o que está explicitamente fora do escopo quando isso já estiver definido.

NÃO invente regras de negócio, permissões, contratos externos ou decisões de produto para preencher lacunas. Use [NEEDS CLARIFICATION: ...] quando uma decisão material não estiver sustentada pelo contexto ou por uma fonte autoritativa.
-->

[Descrever contexto, objetivo e limites da feature]

## User Scenarios & Testing *(mandatory)*

<!--
As user stories devem ser PRIORIZADAS por valor e risco e descritas como jornadas independentes sempre que isso fizer sentido.

Cada história deve ser testável de forma independente no nível de comportamento: implementar apenas uma história deve produzir um incremento demonstrável, salvo dependências fundamentais explicitadas.

P1 = mais importante. Adicione P2, P3 etc. conforme necessário.
-->

### User Story 1 - [Brief Title] (Priority: P1)

[Descrever a jornada em linguagem de produto]

**Why this priority**: [Explicar o valor, risco e motivo da prioridade]

**Independent Test**: [Descrever como provar o valor desta história sem depender de detalhes internos]

**Acceptance Scenarios**:

1. **Given** [estado inicial], **When** [ação], **Then** [resultado observável]
2. **Given** [estado inicial], **When** [ação], **Then** [resultado observável]

---

### User Story 2 - [Brief Title] (Priority: P2)

[Descrever a jornada em linguagem de produto]

**Why this priority**: [Explicar o valor, risco e motivo da prioridade]

**Independent Test**: [Descrever como provar o valor desta história]

**Acceptance Scenarios**:

1. **Given** [estado inicial], **When** [ação], **Then** [resultado observável]

---

[Adicionar histórias conforme necessário]

### Edge Cases

<!--
Registrar condições de borda, erros relevantes, concorrência, permissões, resultados incertos e outras situações que afetem o comportamento da feature.

Não criar edge cases fictícios só para preencher a seção. Priorizar riscos plausíveis sustentados pelo domínio, pela arquitetura existente ou por decisões já tomadas.
-->

- [Edge case relevante]
- [Falha ou condição de borda relevante]

## Requirements *(mandatory)*

### Functional Requirements

<!--
Requisitos funcionais devem ser específicos, testáveis e independentes de implementação sempre que possível.

Use DEVE / NÃO DEVE na redação humana em português. Preserve FR-### como identificador estável.

Assumptions NÃO podem ser usadas para escolher silenciosamente uma regra de negócio, autorização, semântica de dados, comportamento destrutivo ou contrato externo. Nessas situações, use [NEEDS CLARIFICATION: ...].
-->

- **FR-001**: O sistema DEVE [capacidade específica e observável]
- **FR-002**: O sistema NÃO DEVE [comportamento proibido, quando aplicável]

*Exemplo de lacuna material:*

- **FR-003**: O sistema DEVE [NEEDS CLARIFICATION: decisão de produto ou domínio necessária]

### Key Entities *(include if feature involves data)*

- **[Entity 1]**: [O que representa, responsabilidades e relações relevantes sem prescrever implementação]
- **[Entity 2]**: [O que representa e como se relaciona com outras entidades]

## Comportamentos críticos e intenção de verificação *(mandatory)*

<!--
Identifique apenas comportamentos cuja falha teria impacto relevante de negócio, segurança, consistência, histórico, autorização, integração ou fluxo operacional.

A spec identifica O QUE precisa de confiança e POR QUÊ. O plan decide o nível técnico de teste.
-->

| ID | Comportamento crítico | Risco se falhar | Evidência comportamental esperada |
|----|-----------------------|-----------------|-----------------------------------|
| **CB-001** | [Comportamento] | [Impacto] | [O que deve ser observável] |

## Success Criteria *(mandatory)*

<!--
Critérios de sucesso devem ser mensuráveis, verificáveis e preferencialmente independentes de tecnologia. Não invente metas numéricas sem base. Quando uma métrica necessária não estiver definida, use [NEEDS CLARIFICATION: ...].
-->

### Measurable Outcomes

- **SC-001**: [Resultado mensurável e verificável]
- **SC-002**: [Resultado mensurável e verificável]

## Assumptions

<!--
Registrar apenas assumptions de baixo risco que:
  - não criem uma regra de negócio nova;
  - não escolham permissão ou autorização;
  - não inventem contrato externo;
  - não alterem semântica importante de dados;
  - não definam comportamento destrutivo;
  - possam ser revertidas sem mudar o significado da feature.

Toda assumption material deve ser claramente identificada e justificável a partir do contexto. Se não for segura como assumption, use [NEEDS CLARIFICATION: ...] em vez de decidir silenciosamente.
-->

- [Assumption de baixo risco, se existir]
- [Dependência conhecida, se existir]
