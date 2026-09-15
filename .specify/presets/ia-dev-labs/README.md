# IA-dev Labs Spec Kit Preset

Preset experimental do GitHub Spec Kit que materializa a **Constitution de Engenharia do IA-dev Labs v1.0.0** em projetos que usam Spec-Driven Development.

**Versão do preset:** `0.1.0`
**Compatibilidade mínima:** Spec Kit `1.0.5`
**Repositório:** https://github.com/jaimeschettini/ia-dev-engineering
**Status:** desenvolvimento local, ainda não publicado em catálogo

## Objetivo

O preset existe para fazer novos projetos nascerem com os guardrails de engenharia do IA-dev Labs sem copiar manualmente regras entre repositórios.

Ele busca garantir, entre outras coisas:

- regras de negócio explícitas e localizáveis;
- segurança em fronteiras confiáveis;
- arquitetura legível e sem overengineering;
- contratos explícitos;
- consistência, atomicidade e idempotência quando aplicáveis;
- testes escolhidos pela propriedade que precisa ser provada;
- ausência de metas arbitrárias de cobertura;
- TDD opcional, não obrigatório;
- proibição de inventar requisitos ou contratos externos;
- evidência real antes de declarar verificações como concluídas;
- conteúdo humano em português brasileiro, preservando nomes exigidos por ferramentas.

## Estrutura

```text
preset/
├── preset.yml
├── README.md
├── CHANGELOG.md
├── commands/
│   ├── speckit.constitution.md
│   ├── speckit.specify.md
│   └── speckit.tasks.md
└── templates/
    ├── constitution-template.md
    ├── spec-template.md
    ├── plan-template.md
    ├── tasks-template.md
    └── checklist-template.md
```

## Por que existem command wrappers

A versão `0.1.0` não altera o workflow inteiro do Spec Kit. Ela preserva o comportamento core e adiciona três refinamentos obrigatórios por `wrap`:

- `speckit.constitution`: impede que a Constitution de projeto enfraqueça silenciosamente a base global do IA-dev Labs;
- `speckit.specify`: restringe o uso de “reasonable defaults” para que assumptions não virem regras de negócio inventadas;
- `speckit.tasks`: substitui a ideia core de “tests optional” pela estratégia de testes definida pela Constitution e remove qualquer obrigação geral de TDD.

Os demais comandos continuam vindo do Spec Kit core.

## Desenvolvimento local

O preset deve ser testado em um projeto que já tenha sido inicializado com Spec Kit.

A partir da raiz do projeto consumidor:

```bash
specify preset add --dev /caminho/para/ia-dev-engineering/spec-kit/preset
```

Depois:

```bash
specify preset list
```

Verifique a resolução dos templates:

```bash
specify preset resolve constitution-template
specify preset resolve spec-template
specify preset resolve plan-template
specify preset resolve tasks-template
specify preset resolve checklist-template
```

E dos comandos sobrescritos:

```bash
specify preset resolve speckit.constitution
specify preset resolve speckit.specify
specify preset resolve speckit.tasks
```

Para remover o preset após o teste:

```bash
specify preset remove ia-dev-labs
```

## Constitution existente

Instalar ou atualizar um preset **não deve sobrescrever uma Constitution de projeto que já foi editada**.

Para um projeto existente, a adoção da base do IA-dev Labs deve ser deliberada. Depois de instalar o preset, use o fluxo `speckit.constitution` para criar ou atualizar a Constitution do projeto com base no template resolvido, revisando o impacto antes de aceitar a mudança.

Em projetos novos, o objetivo futuro é instalar o preset já durante a inicialização do Spec Kit ou por catálogo, quando o preset estiver publicado.

## Formatação Markdown

A prosa dos artefatos gerados pelo preset não usa hard wrap. Cada parágrafo deve permanecer em uma única linha física no arquivo-fonte, e a quebra visual fica a cargo do editor. Headings, listas, tabelas, blocos de código e outras estruturas Markdown continuam em linhas próprias conforme sua sintaxe.

## Idioma

Os nomes e headings que funcionam como contrato do GitHub Spec Kit são preservados em inglês quando necessário, por exemplo:

- `Core Principles`
- `Governance`
- `Constitution Check`
- `User Scenarios & Testing`
- `Requirements`
- `Functional Requirements`
- `Success Criteria`
- `Measurable Outcomes`
- `Assumptions`
- `Edge Cases`
- `Phase 0: Outline & Research`
- `Phase 1: Design & Contracts`

As instruções, requisitos e demais conteúdos humanos produzidos pelo preset devem permanecer em português brasileiro.

## Licença

A licença ainda não foi definida. O manifest usa `UNLICENSED` nesta fase de desenvolvimento.

Antes de publicar o preset em um catálogo público, definir uma licença explícita e adicionar o arquivo `LICENSE`.

## Critério para v1.0.0

A versão `0.1.0` deve ser validada em pelo menos um projeto real antes de promover o preset para `1.0.0`.

A validação deve confirmar:

1. instalação local sem erros;
2. resolução correta dos cinco templates;
3. registro correto dos três command wrappers;
4. criação/atualização de Constitution sem enfraquecer a base global;
5. geração de `spec.md` sem invenção de regras de negócio;
6. geração de `plan.md` com estratégia de testes explícita;
7. geração de `tasks.md` com tarefas de verificação adequadas ao risco e sem TDD obrigatório;
8. compatibilidade com o agente/integration em uso no projeto piloto.
