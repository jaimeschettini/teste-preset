{CORE_TEMPLATE}

## IA-dev Labs Engineering Override

As regras abaixo refinam as instruções core deste comando e são obrigatórias.

1. A instrução core para usar “reasonable defaults” NÃO autoriza o agente a inventar:
   - regra de negócio;
   - permissão ou autorização;
   - semântica material de dados;
   - comportamento destrutivo;
   - requisito regulatório;
   - contrato de API ou integração externa;
   - métrica quantitativa sem base.
2. `Assumptions` só PODE conter defaults de baixo risco, reversíveis e que não alterem o significado
de produto da feature.
3. Quando uma decisão material não estiver sustentada pelo input, pela Constitution, pela baseline do
projeto ou por fonte autoritativa disponível, usar `[NEEDS CLARIFICATION: ...]` em vez de decidir silenciosamente.
4. A spec DEVE identificar comportamentos críticos e o risco associado, mas NÃO DEVE prescrever uma
arquitetura ou nível técnico de teste antes do `plan.md`.
5. Requisitos DEVEM ser específicos, testáveis e redigidos em português brasileiro, preservando
identificadores `FR-###`, `SC-###`, nomes de headings e outros contratos do GitHub Spec Kit.
6. O agente NÃO DEVE preencher seções com conteúdo fictício apenas para eliminar placeholders.
7. Prosa em Markdown NÃO DEVE usar hard wrap. Cada parágrafo de prosa DEVE permanecer em uma única linha física; a quebra visual deve ser delegada ao editor. Headings, listas, tabelas, blocos de código e outras estruturas Markdown permanecem em linhas próprias conforme sua sintaxe.
