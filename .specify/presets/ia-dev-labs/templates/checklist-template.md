# [CHECKLIST TYPE] Checklist: [FEATURE NAME]

<!-- IA-dev Labs formatting: prosa em Markdown não usa hard wrap; mantenha cada parágrafo em uma única linha física e deixe a quebra visual para o editor. -->

**Purpose**: [Descrição breve do aspecto de qualidade dos requisitos que será revisado]

**Created**: [DATE]

**Feature**: [Link para spec.md ou documentação relevante]

**Note**: Este checklist é gerado pelo `$speckit-checklist` para revisar a QUALIDADE DOS REQUISITOS, não para confirmar que a implementação funciona.

**Review Ownership**: Marque `[x]` apenas quando o critério de qualidade do requisito tiver sido efetivamente revisado e satisfeito.

**Marker Semantics**: `[x]` significa “requisito revisado e adequado”. NÃO significa “implementação concluída”.

<!--
============================================================================ IMPORTANT: Os itens abaixo são apenas exemplos.

O $speckit-checklist DEVE substituí-los por itens reais baseados em:
  - solicitação específica do checklist;
  - spec.md;
  - plan.md quando disponível;
  - tasks.md quando relevante para rastreabilidade.

O checklist deve testar os REQUISITOS COMO TEXTO:
  - completude;
  - clareza;
  - consistência;
  - mensurabilidade;
  - ausência de assumptions materiais escondidas;
  - edge cases e falhas relevantes;
  - critérios de aceitação;
  - rastreabilidade de comportamentos críticos.

NÃO manter sample items no arquivo final. ============================================================================
-->

## [Category 1]

- [ ] CHK001 [Critério claro de qualidade do requisito]
- [ ] CHK002 [Critério claro de completude ou ausência de ambiguidade]

## [Category 2]

- [ ] CHK003 [Critério sobre edge cases, riscos ou falhas relevantes]
- [ ] CHK004 [Critério sobre mensurabilidade ou rastreabilidade]

## Notes

- Marcar `[x]` somente após revisão real.
- Deixar desmarcado quando houver necessidade de esclarecimento ou correção.
- Não transformar o checklist em plano de testes da implementação.
- Assumptions materiais não podem esconder decisões de negócio ou contratos externos não validados.
- `$speckit-implement` pode ler o estado do checklist como gate; o checklist não deve ser marcado por
conveniência para desbloquear implementação.
- IDs `CHK###` devem ser únicos e sequenciais.
