# [PROJECT_NAME] Constitution

<!-- IA-dev Labs formatting: prosa em Markdown não usa hard wrap; mantenha cada parágrafo em uma única linha física e deixe a quebra visual para o editor. -->

**Base de engenharia**: IA-dev Labs v1.0.0
**Contexto do projeto**: [PROJECT_CONTEXT]

Esta Constitution materializa os princípios de engenharia do IA-dev Labs no contexto deste projeto. Ela é completa e autocontida: o projeto NÃO DEVE depender dinamicamente da Constitution global para ser compreendido, auditado ou desenvolvido.

Regras marcadas como **DEVE** ou **NÃO DEVE** são obrigatórias. Regras marcadas como **DEVERIA** representam o padrão esperado e só podem ser desviadas com justificativa explícita e decisão consciente no contexto do projeto.

## Core Principles

### I. Regras de negócio explícitas e localizáveis

Regras de domínio DEVEM, por padrão, residir no código da aplicação e permanecer claramente localizáveis. Infraestrutura, persistência, componentes de interface, handlers, endpoints e mecanismos equivalentes NÃO DEVEM se tornar o local acidental de regras de negócio que possam ser expressas de forma mais clara na camada responsável pelo domínio ou caso de uso.

Endpoints, handlers, controllers, Server Functions ou mecanismos equivalentes DEVERIAM permanecer finos: sua responsabilidade principal é autenticar, autorizar, validar entradas, orquestrar o caso de uso e traduzir a resposta.

Stored procedures, database functions, triggers ou outros mecanismos executados junto à persistência só DEVEM conter regra de domínio quando houver vantagem técnica concreta e documentada, como segurança, integridade, atomicidade, concorrência ou operação transacional próxima aos dados. Esses mecanismos NÃO DEVEM ser usados apenas para deslocar lógica da aplicação ou concentrar casos de uso em grandes blocos de código de banco.

Requisitos, regras de negócio e contratos externos NÃO DEVEM ser inventados para preencher lacunas. Quando uma informação necessária estiver ausente, incerta ou depender de fonte ainda não validada, a lacuna DEVE permanecer explícita até que exista evidência ou decisão suficiente.

**Justificativa:** regras explícitas e localizáveis reduzem acoplamento, facilitam testes, tornam mudanças mais seguras e impedem que o comportamento real do software fique distribuído de forma implícita entre interface, infraestrutura e persistência.

### II. Fronteiras de confiança e segurança explícitas

Clientes, browsers, aplicativos móveis e outros ambientes controlados pelo usuário DEVEM ser tratados como fronteiras não confiáveis quando participarem da arquitetura. Decisões de autorização, identidade, papel, escopo ou permissão NÃO DEVEM depender exclusivamente de estado, parâmetros, metadata ou flags fornecidos pelo cliente.

Quando o sistema possuir autenticação, autorização ou operações privilegiadas, essas garantias DEVEM ser aplicadas em uma fronteira confiável da arquitetura. Operações privilegiadas DEVEM validar explicitamente o ator, o escopo e as permissões necessárias quando esses conceitos forem aplicáveis.

Segredos, chaves privadas, tokens, senhas e credenciais privilegiadas NÃO DEVEM ser expostos a clientes não confiáveis, versionados no repositório ou registrados em logs. Quando uma credencial privilegiada puder contornar controles normais de acesso, seu uso DEVE ser acompanhado de autorização explícita no código confiável.

Áreas privadas NÃO DEVEM depender de ocultação, `robots.txt`, `noindex`, `nofollow` ou mecanismo semelhante como proteção de segurança. Esses mecanismos PODEM complementar a experiência ou reduzir indexação quando aplicável, mas nunca substituem autenticação e autorização.

**Justificativa:** controles visuais e convenções de cliente melhoram a experiência, mas não constituem uma fronteira de segurança. As garantias precisam permanecer válidas diante de clientes modificados, chamadas diretas e entradas malformadas ou maliciosas.

### III. Legibilidade humana, coesão e arquitetura simples

O código DEVE ser escrito prioritariamente para ser compreendido e mantido por seres humanos, não apenas para funcionar, compilar ou satisfazer ferramentas automáticas. Código correto, mas desnecessariamente difícil de compreender, não atende ao padrão de qualidade do IA-dev Labs.

Nomes de funções, variáveis, tipos, módulos e componentes DEVEM comunicar intenção e domínio com clareza. Unidades de código DEVEM possuir responsabilidades coesas e limites compreensíveis. Domínio, interface e infraestrutura DEVEM permanecer separados quando possuírem responsabilidades distintas.

Dependências DEVEM favorecer uma direção que preserve a independência das regras centrais em relação a detalhes externos quando essa separação trouxer benefício concreto de compreensão, teste, evolução ou substituição.

Abstrações, interfaces, camadas, factories, repositories, wrappers, services, adapters e outros padrões arquiteturais só DEVEM ser introduzidos quando resolverem um problema presente e identificável. Complexidade arquitetural preventiva ou especulativa NÃO DEVE ser criada. Duplicação pequena e explícita PODE ser preferível a uma abstração prematura que torne o código mais difícil de compreender.

Comentários DEVEM explicar contexto, decisões, restrições ou razões não evidentes. Eles NÃO DEVEM compensar nomes ruins nem narrar literalmente o que o código já expressa.

Este princípio adota os objetivos úteis de Clean Code e Clean Architecture, especialmente legibilidade, separação de responsabilidades, baixo acoplamento e facilidade de mudança, sem impor arquitetura em camadas específica ou arquitetura cerimonial.

**Justificativa:** agentes de IA conseguem produzir rapidamente grandes volumes de código funcional. Essa velocidade não pode ser comprada ao custo de uma base opaca para revisão humana, depuração e manutenção futura.

### IV. Tipagem forte e contratos explícitos

Estados, dados, entradas, saídas e erros DEVEM possuir contratos explícitos e coerentes com o domínio e com as fronteiras da aplicação.

Quando a linguagem ou plataforma oferecer tipagem estática ou mecanismos equivalentes de validação estrutural, o projeto DEVE utilizar uma configuração suficientemente estrita para detectar inconsistências cedo. O sistema de tipos ou validação NÃO DEVE ser contornado apenas para eliminar erros de compilação, análise ou integração.

Casts inseguros, coerções amplas, tipos genéricos sem necessidade, uso indiscriminado de valores dinâmicos ou mecanismos equivalentes DEVEM ser excepcionais, tecnicamente necessários e justificáveis. Sempre que possível, o contrato DEVE ser corrigido na origem ou propagado corretamente pela arquitetura.

Fluxos de controle e regras de negócio DEVEM usar códigos, tipos, estados, enums, discriminadores ou estruturas explícitas quando houver uma representação adequada. Texto destinado a humanos, como mensagens de erro, NÃO DEVE funcionar como contrato lógico quando houver alternativa estruturada.

Código morto, contratos não utilizados e inconsistências estáticas DEVEM ser detectados cedo pelas ferramentas disponíveis na stack.

**Justificativa:** contratos explícitos transformam uma parte relevante dos erros em falhas detectáveis antes da execução e tornam a intenção do sistema mais clara para humanos e agentes.

### V. Consistência, atomicidade e idempotência

Quando uma operação exigir múltiplas alterações que precisam ser concluídas em conjunto para manter o sistema correto, a implementação DEVE preservar essa consistência e garantir atomicidade quando isso for tecnicamente apropriado.

Transações DEVEM ser coordenadas na camada responsável pelo caso de uso ou persistência, preservando funções pequenas, coesas e legíveis. A necessidade de atomicidade NÃO DEVE, por si só, deslocar regras de negócio para stored procedures, database functions ou mecanismos equivalentes.

Transações de persistência NÃO DEVEM permanecer abertas durante chamadas a serviços externos, salvo necessidade técnica excepcional e explicitamente justificada.

Toda migração que altere estrutura ou dados persistidos DEVE possuir uma estratégia explícita de rollback ou recuperação segura. Quando a reversão automática não for tecnicamente segura ou possível, essa limitação DEVE ser identificada antes da execução e acompanhada de uma estratégia documentada de recuperação, restauração ou roll-forward.

Migrações destrutivas ou potencialmente irreversíveis NÃO DEVEM ser executadas sem avaliação explícita do impacto sobre dados existentes e da estratégia de recuperação.

Quando um fluxo atravessar sistemas que não compartilham uma transação, falhas parciais DEVEM ser tratadas explicitamente por recuperação, compensação, reconciliação ou mecanismo equivalente. O sistema NÃO DEVE fingir atomicidade onde ela não existe.

Operações sujeitas a retry, concorrência, timeout ou resultado incerto DEVEM possuir estratégia de idempotência, deduplicação ou reconciliação adequada ao risco. Estados intermediários relevantes DEVEM ser recuperáveis de forma determinística quando sua perda puder causar efeitos duplicados, corrupção ou inconsistência operacional.

**Justificativa:** efeitos distribuídos, concorrência e integrações externas geram modos de falha que não desaparecem por convenção. Consistência precisa ser uma propriedade projetada, e não uma expectativa implícita.

### VI. Comportamento crítico respaldado por verificação automatizada

Comportamentos críticos de negócio, segurança, persistência e fluxos operacionais DEVEM ser protegidos por testes automatizados adequados ao risco. Uma alteração não é considerada completa apenas porque compila, passa por análise estática ou funciona em uma verificação manual pontual.

Os testes DEVEM validar contratos e comportamentos observáveis e NÃO DEVEM reproduzir desnecessariamente detalhes internos da implementação. Correções de bugs DEVEM, quando tecnicamente viável, incluir teste de regressão que reproduza o cenário e falhe caso o comportamento incorreto seja reintroduzido.

A estratégia de testes DEVE utilizar o nível mais simples capaz de fornecer confiança suficiente, complementando-o com testes de integração, contrato ou end-to-end quando a propriedade a ser garantida atravessar fronteiras que um teste isolado não consegue validar.

Não existe meta constitucional de 100% de cobertura nem percentual arbitrário de cobertura como critério isolado de qualidade. Cobertura PODE ser usada como sinal, nunca como prova suficiente.

**Justificativa:** em um processo com forte participação de agentes de IA, a confiabilidade não pode depender de revisão humana linha a linha. A automação de verificação é um guardrail essencial contra regressões e falsas suposições de correção.

### VII. Responsabilidade verificável dos agentes de IA

Código, documentação e decisões produzidos ou alterados por agentes de IA estão sujeitos aos mesmos requisitos de qualidade, segurança e verificação aplicáveis ao trabalho humano.

Agentes NÃO DEVEM inventar requisitos, regras de negócio, contratos externos, resultados de testes, comportamento observado ou evidência de execução. Lacunas relevantes DEVEM ser explicitadas e, quando necessário, trazidas para decisão humana.

Agentes NÃO DEVEM assumir permissão para implementar, alterar código, modificar áreas protegidas ou executar ações externas apenas por possuírem capacidade técnica para fazê-lo. Responsabilidades, permissões de escrita e limites operacionais DEVEM ser definidos pelo projeto e respeitados durante todo o trabalho.

Ao concluir uma unidade relevante de implementação ou verificação, o agente responsável DEVE informar a evidência real produzida, incluindo testes executados, comandos relevantes, resultados observados e desvios conhecidos quando aplicável.

O agente NÃO DEVE declarar que testes passaram, que uma verificação foi executada ou que um comportamento foi validado sem possuir evidência real da respectiva execução.

**Justificativa:** autonomia útil exige rastreabilidade. A confiança no agente deve vir de contratos claros, resultados verificáveis e limites explícitos, não de afirmações não comprovadas.

## Princípios e restrições específicos do projeto

<!--
Inclua aqui somente guardrails transversais explicitamente definidos pelo usuário ou inequivocamente sustentados pelo contexto fornecido.

NÃO transforme funcionalidades do produto, ausência de requisitos, preferências presumidas, escolhas de stack ou estratégia de testes em princípios constitucionais.

Se nenhum guardrail específico tiver sido definido, escreva exatamente: "Nenhum guardrail específico do projeto foi estabelecido neste momento."
-->

[PROJECT_SPECIFIC_PRINCIPLES_AND_CONSTRAINTS]

## Restrições técnicas e de segurança

- Segredos e credenciais DEVEM permanecer fora do código versionado e de logs.
- Privilégios elevados DEVEM ser restritos a fronteiras confiáveis e usados com autorização explícita.
- Dependências externas, bibliotecas e serviços críticos DEVEM ter necessidade e papel compreensíveis.
- Mocks DEVEM ser determinísticos, reproduzíveis e claramente identificados como simulação.
- Contratos externos relevantes DEVEM se basear em documentação, schemas, APIs ou outras fontes
autoritativas vigentes.
- Dados históricos que representem fatos já ocorridos NÃO DEVEM ser reescritos silenciosamente quando
isso comprometer auditoria, rastreabilidade ou correção do domínio.
- Áreas privadas ou administrativas DEVEM possuir proteção real de autenticação e autorização quando
esses conceitos forem aplicáveis.

[PROJECT_SPECIFIC_TECHNICAL_CONSTRAINTS]

## Fluxo de desenvolvimento e critérios de qualidade

### Spec-Driven Development e responsabilidades dos agentes

O projeto DEVE seguir Spec-Driven Development quando esta Constitution estiver sendo usada como parte do workflow do GitHub Spec Kit.

Responsabilidades, permissões de escrita e limites operacionais de agentes e ferramentas DEVEM ser definidos no próprio projeto, preferencialmente em `AGENTS.md` quando essa convenção for compatível com as ferramentas adotadas.

Documentação, Constitutions, especificações, planos, tarefas, critérios de aceitação, prompts próprios para agentes, descrições de commits e comunicação humana do desenvolvimento DEVEM ser escritos em português brasileiro. Identificadores de código, contratos de APIs, comandos, nomes e estruturas exigidos por ferramentas DEVEM preservar o idioma e a forma definidos pelos respectivos contratos.

### Critérios de qualidade de arquitetura e legibilidade

Antes de uma mudança relevante ser considerada pronta para implementação ou concluída, o responsável DEVE verificar:

1. se as regras de domínio permanecem claras e localizáveis;
2. se autenticação, autorização e segredos permanecem em fronteiras confiáveis quando aplicável;
3. se a solução introduz apenas a complexidade arquitetural necessária para o problema atual;
4. se nomes, funções, módulos, componentes e dependências permanecem compreensíveis;
5. se contratos de tipos, dados e erros são explícitos;
6. se concorrência, falhas parciais, retries e estados intermediários relevantes possuem estratégia;
7. se integrações externas dependem de contratos conhecidos em vez de suposições inventadas;
8. se os comportamentos críticos afetados possuem estratégia de teste adequada;
9. se as evidências de verificação correspondem ao que realmente foi executado.

Qualquer necessidade de violar conscientemente um princípio desta Constitution DEVE ser explicitada antes da implementação, incluindo necessidade concreta, impacto, alternativa mais simples considerada e justificativa para sua rejeição. A exceção DEVE ser submetida à decisão humana explícita responsável pelo projeto.

### Estratégia obrigatória de testes

Toda especificação ou plano de implementação de mudança relevante DEVE identificar os comportamentos críticos afetados e indicar o nível de teste adequado. A escolha NÃO DEVE ser feita por hábito ou meta de cobertura, mas pela propriedade que precisa ser provada.

- **Domínio/unidade**: usar quando a regra puder ser verificada sem banco, browser, rede ou
infraestrutura externa.
- **Integração**: usar quando a garantia depender da interação real entre persistência, autorização,
constraints, transações, migrations, filas, caches ou infraestrutura equivalente.
- **Componente/UI**: quando houver interface, usar quando for o nível mais simples para validar
comportamento visível sem executar o fluxo completo.
- **E2E**: usar seletivamente para fluxos de alto valor ou risco que atravessem múltiplas fronteiras
reais e não possam ser suficientemente provados por níveis inferiores.
- **Contrato/integrações externas**: verificar localmente serialização, parsing, validação,
mapeamento de erros e contratos sempre que aplicável.

Um teste de integração NÃO DEVE substituir por mock justamente a camada cuja garantia está sendo validada.

TDD NÃO é obrigatório. PODE ser usado quando melhorar segurança ou clareza.

Quando uma propriedade crítica não puder ser testada automaticamente, a limitação, o risco e a verificação alternativa DEVEM ser registrados explicitamente e submetidos à decisão responsável pelo projeto.

## Governance

Esta Constitution é a autoridade de engenharia transversal deste projeto e foi derivada da Constitution de Engenharia do IA-dev Labs v1.0.0.

Regras específicas do projeto PODEM acrescentar restrições e contextualizar a materialização técnica, mas NÃO DEVEM reduzir silenciosamente as garantias da base declarada. Qualquer divergência consciente DEVE ser explícita, justificada e aprovada pela pessoa responsável pelo projeto.

Alterações à Constitution exigem decisão explícita, atualização do Sync Impact Report e revisão das consequências sobre especificações, planos, tarefas, instruções de agentes e implementação existente.

O versionamento segue Semantic Versioning para governança:

- **MAJOR**: remoção de princípio ou redefinição incompatível de uma garantia existente;
- **MINOR**: novo princípio, nova seção ou expansão material das obrigações;
- **PATCH**: clarificações, correções de texto ou refinamentos sem mudança semântica relevante.

Todo plano criado pelo GitHub Spec Kit DEVE executar o `Constitution Check` antes da `Phase 0: Outline & Research` e revalidá-lo após a `Phase 1: Design & Contracts`.

A conformidade NÃO DEVE ser avaliada apenas pela existência de documentos, testes ou checklists. A revisão DEVE verificar se a intenção dos princípios foi preservada no comportamento, na arquitetura e nas evidências produzidas.

**Version**: [CONSTITUTION_VERSION] | **Ratified**: [RATIFICATION_DATE] | **Last Amended**: [LAST_AMENDED_DATE]
