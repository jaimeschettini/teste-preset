{CORE_TEMPLATE}

## IA-dev Labs Engineering Override

As regras abaixo substituem explicitamente qualquer orientação core incompatível sobre testes e ordenação de implementação.

1. A frase core “Tests are OPTIONAL” NÃO se aplica quando a Constitution, a spec ou o plan
identificarem comportamentos críticos que exigem verificação automatizada.
2. O `tasks.md` DEVE incluir as tasks de teste/verificação definidas na estratégia do `plan.md`.
3. TDD NÃO é obrigatório. NÃO exigir que testes sejam escritos e falhem antes da implementação, salvo
quando o usuário, a Constitution específica ou o plan tiverem escolhido TDD para aquela mudança.
4. O nível de teste DEVE seguir a propriedade que precisa ser provada:
domínio/unidade, integração, componente/UI, E2E ou contrato conforme aplicável.
5. NÃO duplicar em E2E regras já provadas de forma mais simples e estável em nível inferior.
6. Cada user story ou incremento relevante DEVE possuir tasks suficientes para produzir evidência
real de conclusão antes de ser marcado como pronto.
7. NÃO inventar layers, models, services, repositories, adapters ou outras abstrações durante a
decomposição. Usar a estrutura real definida no `plan.md`.
8. Tasks DEVEM ser concretas, citar caminhos reais quando alterarem arquivos e evitar descrições vagas.
9. O agente NÃO DEVE marcar verificação como concluída sem execução real e resultado observado.
10. Conteúdo humano do `tasks.md` DEVE permanecer em português brasileiro, preservando IDs e
convenções exigidas pelo GitHub Spec Kit.
11. Prosa em Markdown NÃO DEVE usar hard wrap. Cada parágrafo de prosa DEVE permanecer em uma única linha física; a quebra visual deve ser delegada ao editor. Headings, listas, tabelas, blocos de código e outras estruturas Markdown permanecem em linhas próprias conforme sua sintaxe.
