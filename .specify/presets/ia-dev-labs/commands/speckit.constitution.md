{CORE_TEMPLATE}

## IA-dev Labs Engineering Override

As regras abaixo complementam e, em caso de ambiguidade, refinam o workflow core.

1. O `constitution-template` resolvido pelo preset contém a base de engenharia do IA-dev Labs v1.0.0.
Os sete Core Principles são a baseline obrigatória para projetos que adotam este preset.
2. A Constitution do projeto DEVE permanecer completa e autocontida. Ela PODE acrescentar ou
contextualizar guardrails específicos de produto, domínio ou stack.
3. O agente NÃO DEVE remover, enfraquecer ou contradizer silenciosamente uma garantia da base global.
Se o usuário pedir uma divergência, o agente DEVE:
   - explicitar a divergência;
   - explicar o impacto;
   - obter decisão humana explícita;
   - registrar a divergência no Sync Impact Report e na Governance quando ela for aprovada.
4. A versão da base de engenharia usada pelo projeto DEVE permanecer registrada na Constitution.
5. Conteúdo humano DEVE ser escrito em português brasileiro, preservando nomes, headings, comandos e
estruturas cujo formato seja exigido pelo GitHub Spec Kit ou por outra ferramenta.
6. Não modificar templates do preset ou do core durante o workflow de Constitution. Escrever apenas
`.specify/memory/constitution.md`, conforme o contrato core.
7. O agente NÃO DEVE inferir ou criar princípios, restrições ou guardrails específicos do projeto apenas a partir da descrição funcional do produto. Funcionalidades, prioridades, comportamentos críticos, escolhas de stack e estratégia de testes pertencem à spec e ao plan, salvo quando o usuário os tiver explicitamente definido como restrições transversais do projeto. Quando não houver guardrails específicos suficientemente definidos, a seção correspondente DEVE declarar que nenhum guardrail específico do projeto foi estabelecido neste momento, em vez de inventá-los.
8. Prosa em Markdown NÃO DEVE usar hard wrap. Cada parágrafo de prosa DEVE permanecer em uma única linha física; a quebra visual deve ser delegada ao editor. Headings, listas, tabelas, blocos de código e outras estruturas Markdown permanecem em linhas próprias conforme sua sintaxe.
