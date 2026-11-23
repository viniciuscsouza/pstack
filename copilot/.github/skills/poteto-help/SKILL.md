---
name: poteto-help
description: Guia o usuário pela configuração do pstack, pelo /poteto-mode, e pela escolha da skill, do playbook ou do princípio para uma tarefa. Digite /poteto-help com uma pergunta.
---

# Ajuda do poteto

Responda a pergunta do usuário sobre o pstack, entregue um prompt que ele possa enviar, e linke o arquivo de onde a resposta veio. Numa pergunta de ajuda, não comece o trabalho. O usuário perguntou como, e uma execução do pstack gasta tokens de verdade, então deixe ele enviar o prompt.

Uma mensagem que pede trabalho, como "use o pstack para corrigir este bug", não é pergunta de ajuda. Leia [`poteto-mode`](../poteto-mode/SKILL.md), faça o trabalho sob ela, e mencione uma vez que um Custom Mode mantém ela ligada.

Este arquivo mapeia perguntas para as skills e páginas do guia que guardam as respostas. Esses arquivos são donos dos detalhes. Leia o arquivo para o qual você roteia antes de citá-lo, e confie nele quando ele discordar deste mapa. Os links aqui apontam para dentro do plugin instalado, que o usuário pode não conseguir abrir, então entregue a cópia pública do arquivo, `https://github.com/cursor/plugins/blob/main/pstack/` seguida do caminho dele.

## Descubra o que ele precisa

Infera a necessidade pela mensagem e pela conversa. Uma situação nomeada, como "qual skill revisa um PR?", vai direto para a sua seção. Se a necessidade ainda não estiver clara, faça uma pergunta de múltipla escolha com estas opções, depois responda só a seção que ele escolher.

- Configurar
- Começar uma tarefa com `/poteto-mode`
- Escolher uma skill para uma situação
- Corrigir uma execução que deu errado
- Deixar o pstack com a minha cara

Verifique o estado que muda a resposta, e mencione ele só quando mudar.

- Sem `~/.cursor/rules/pstack-models.mdc`, o `/setup-pstack` não rodou para este usuário, então todo papel usa o modelo padrão.
- Sem skill `verify-*` ou outro harness de app no projeto, os agentes não têm um jeito roteirizado de dirigir o app. Mencione `/create-verification-skill` quando a pergunta for sobre provar que uma mudança funciona.
- Sem skill de verificação formal no projeto, nenhuma propriedade é checada além do que os testes alcançam. Mencione `/create-formal-verification-skill` quando a pergunta for sobre uma corrida, um invariante, ou outro caso que os testes raramente pegam. Quando existe uma e o código que ela cobre mudou depois do commit que ela registra como último em sincronia, mencione `/maintain-formal-verification-skill` se a pergunta tocar esse código.

Quando a regra de modelo está faltando e isso importa, pergunte se o usuário quer escolher um modelo por papel e um orçamento de raciocínio agora. Isso importa quando o usuário é novo, a pergunta é sobre configuração ou custo, ou a resposta depende de quais modelos rodam. Pergunte no máximo uma vez por chat. Se a necessidade também não estiver clara, faça as duas perguntas juntas. Ofereça duas escolhas.

- Agora. Entregue `/setup-pstack` para ele digitar, e responda a pergunta dele também.
- Depois. Responda a pergunta dele, e acrescente uma linha dizendo que todo papel mantém o modelo padrão até ele rodar `/setup-pstack`.

## Configure

1. Instale com `/add-plugin pstack` no chat, ou em Customize na barra lateral.
2. Rode [`/setup-pstack`](../setup-pstack/SKILL.md). Ele pede um orçamento de raciocínio, mapeia um modelo para cada papel, e escreve uma regra. A regra vale para chats novos. Depois ele oferece uma vez cada a geração de uma skill de verificação e de uma skill de verificação formal para o projeto, a menos que o projeto já tenha uma.
3. Comece uma tarefa de verdade com `/poteto-mode`, um objetivo, e uma checagem que possa passar ou falhar.

Instalar não muda nada até o usuário invocar uma skill. Só o `/setup-pstack` carrega a partir das palavras do usuário. O [README](../../../README.md) e a [página 1 do guia](../../../docs/guide/01-setup.md) têm os detalhes. Ofereça escrever o primeiro prompt junto com ele, conforme [`references/prompting.md`](references/prompting.md).

Se o custo é a preocupação, diga para onde vão os tokens e como gastar menos. O pstack gasta tokens extras em subagentes e painéis de revisão. Rode `/setup-pstack` de novo e escolha um orçamento menor ou modelos mais baratos. Um papel definido como `auto` ou `inherit-parent` roda no modelo do chat, o que economiza tokens quando o chat roda em Auto ou num modelo mais barato. Uma lista de painel mais curta roda menos subagentes, um para cada entrada. Guarde `/poteto-mode` para trabalho que precisa de rigor.

O pstack de origem é um plugin do Cursor e também instala no Claude Code com `/plugin marketplace add cursor/plugins`, depois `/plugin install pstack@cursor-plugins`. Nessa instalação do plugin de origem, cada comando leva o prefixo `pstack:`, como `/pstack:poteto-mode`. O [adaptador de GitHub Copilot](../../../docs/harness/copilot.md) mapeia os nomes do Cursor, e um passo que precisa de recurso exclusivo do Cursor, como Bugbot ou Automations, é pulado com uma nota de uma linha. Neste pacote adaptado o namespace é próprio: não use o identificador do plugin de origem aqui.

Outras ferramentas conseguem ler as skills, porque usam o formato Agent Skills. Mas a maioria das skills de fluxo, incluindo `/poteto-mode`, `/how` e `/teach`, cria subagentes do Cursor, alguns com modelos por papel, e Custom Modes e `/loop` são recursos do Cursor, então essas partes podem não funcionar lá.

## Comece uma tarefa com `/poteto-mode`

O `/poteto-mode` casa a tarefa com um playbook, copia os passos do playbook para a lista de tarefas, e roda as outras skills conforme os passos precisam. Um bom prompt declara o objetivo e como saber que acabou. Ele não lista skills, porque uma sequência escrita à mão tende a perder ou reordenar passos que o playbook manteria. Leia [`references/prompting.md`](references/prompting.md) antes de ajudar a escrever um. A [página 2 do guia](../../../docs/guide/02-poteto-mode.md) tem exemplos.

Se o `/poteto-mode` fica ligado depende de como o usuário inicia ele.

- Enter em `/poteto-mode` anexa a skill a uma mensagem. Ela vai sumindo conforme o chat avança.
- Option+Enter no Mac ou Alt+Enter no Windows, ou Use as Mode na entrada da skill, transforma em Custom Mode. Ele fica no contexto a cada turno até o usuário sair do modo, e fica fora de turnos casuais.
- Os docs do Cursor listam Custom Modes na Agents Window e na CLI. Em outros lugares, comece cada tarefa nova com `/poteto-mode`.

Linke os [docs de skills do Cursor](https://cursor.com/docs/skills) quando isso aparecer. No meio do chat, "tarefa nova" faz o modo casar com um playbook novo. O `/poteto-mode` já usa `poteto-agent` para os subagentes que os passos do playbook criam. Para ter o mesmo estilo num subagente seu, crie ele com `subagent_type: "poteto-agent"`.

## Escolha uma skill

A resposta padrão é `/poteto-mode`, que roda a maioria das outras quando os passos precisam. Nomeie uma skill direto quando o usuário quer mais ou menos de algo do que o playbook entrega. Leia a skill antes de recomendar, e dê um prompt de exemplo.

| O usuário quer | Skill |
|---|---|
| Fazer qualquer tarefa não trivial com rigor | [`/poteto-mode`](../poteto-mode/SKILL.md) |
| Checar que o agente entendeu o objetivo e o problema antes de começar | [`/in-your-own-words`](../in-your-own-words/SKILL.md) |
| Ter um prompt reescrito mais fundo, não maior, antes de enviar. O que um profissional de ponta acrescentaria, o material bruto e os métodos dele, e uma checagem externa. Ele devolve o prompt e não faz a tarefa | [`/sharpen`](../sharpen/SKILL.md) |
| Saber como o código funciona hoje, ou onde código novo deve morar | [`/how`](../how/SKILL.md) |
| Saber por que o código tem essa forma, ou de onde veio um número | [`/why`](../why/SKILL.md) |
| Entender uma mudança ou subsistema, explicado de forma simples | [`/teach`](../teach/SKILL.md) |
| Se atualizar sobre o próprio trabalho recente num tema | [`/recall`](../recall/SKILL.md) |
| Se atualizar sobre o que mudou neste chat desde a última mensagem, em palavras simples e visuais pequenos, com todo item esperando por ele listado e decidível só pela resposta | [`/ketchup`](../ketchup/SKILL.md) |
| Se atualizar como o `/ketchup`, num vídeo curto de slides com legenda, com as decisões pendentes como texto embaixo | [`/ketchup-video`](../ketchup-video/SKILL.md) |
| Saber o que um diff pequeno pode quebrar fora dele | [`/blast-radius`](../blast-radius/SKILL.md) |
| Fechar tipos e forma de módulo antes de código que atravessa uma fronteira de função | [`/architect`](../architect/SKILL.md) |
| Ter várias tentativas de um mesmo briefing, fundidas na melhor | [`/arena`](../arena/SKILL.md) |
| Rodar checagens paralelas sobre fatias, ou workers em corrida, como agentes de nuvem | [`/swarm`](../swarm/SKILL.md) |
| Ter modelos diferentes revisando um diff e tentando quebrá-lo | [`/interrogate`](../interrogate/SKILL.md) |
| Corrigir um bug com teste primeiro quando existe um teste local barato | [`/tdd`](../tdd/SKILL.md) |
| Aplicar as regras de TypeScript em trabalho com `.ts` ou `.tsx` | [`/typescript-best-practices`](../typescript-best-practices/SKILL.md) |
| Tirar comentários antes da revisão, usando um revisor que não os escreveu | [`/no-comments`](../no-comments/SKILL.md) |
| Limpar marcas de IA da prosa | [`/unslop`](../unslop/SKILL.md) |
| Escrever docs, um RFC, um README, uma descrição de PR ou uma mensagem de commit num padrão | [`/technical-writing`](../technical-writing/SKILL.md) |
| Ouvir a última resposta de novo em palavras simples | [`/bro`](../bro/SKILL.md) |
| Dar aos agentes um jeito roteirizado de dirigir o app e provar comportamento | [`/create-verification-skill`](../create-verification-skill/SKILL.md) |
| Realinhar uma skill de verificação e o mapa de funcionalidades dela com o app | [`/maintain-verification-skill`](../maintain-verification-skill/SKILL.md) |
| Verificar formalmente uma corrida, invariante ou outra propriedade que os testes raramente alcançam | [`/create-formal-verification-skill`](../create-formal-verification-skill/SKILL.md) |
| Realinhar uma verificação formal depois que o código que ela cobre mudou, e descobrir se uma falha é desvio ou bug | [`/maintain-formal-verification-skill`](../maintain-formal-verification-skill/SKILL.md) |
| Mostrar a um PM ou designer o que o usuário vê como um gráfico checado por um modelo TLA+, com o caminho mais curto até cada estado que quebra uma regra | [`/statechart`](../statechart/SKILL.md) |
| Avaliar com rigor um número de performance antes de relatar ou agir sobre ele | [`/benchmark-checklist`](../benchmark-checklist/SKILL.md) |
| Rodar uma mudança grande ou transversal, ou uma para revisar depois de se ausentar | [`/figure-it-out`](../figure-it-out/SKILL.md) |
| Manter um log de decisões durante uma execução, e revisar depois | [`/show-me-your-work`](../show-me-your-work/SKILL.md) |
| Escolher um modelo por papel e um orçamento de raciocínio | [`/setup-pstack`](../setup-pstack/SKILL.md) |
| Transformar os próprios hábitos de trabalho numa skill de modo pessoal | [`/automate-me`](../automate-me/SKILL.md) |
| Transformar o que uma tarefa concluída ensinou em edições de skill | [`/reflect`](../reflect/SKILL.md) |
| Impedir que os agentes repitam os mesmos erros neste repo | [`/correct`](../correct/SKILL.md) |
| Construir uma página cujos botões acordam um Grok Bot via webhook | [`/make-bot-ui`](../make-bot-ui/SKILL.md) |
| Se localizar no pstack | `/poteto-help` |

Se um diretório de skill ao lado deste estiver faltando na tabela, leia o frontmatter dele e roteie pela descrição. Os diretórios `principle-*` estão cobertos em princípios abaixo.

Casos próximos.

- O `/how` explica o que o código faz. O `/why` explica os motivos. O `/teach` roda um ou os dois e explica o resultado de forma simples.
- O `/arena` dá o mesmo briefing a todo worker e funde as melhores partes. O `/swarm` divide o trabalho em fatias ou numa corrida e devolve um relatório.
- O `/interrogate` revisa o diff. O `/blast-radius` procura quebra fora do diff e prova o fato que torna a mudança segura.
- O `/recall` reconstrói contexto entre chats recentes. O `/ketchup` cobre só este chat desde a última mensagem do usuário. Retomar um chat ou branch específico é o playbook Session pickup.
- O `/figure-it-out` desenha uma execução rigorosa. O playbook Autonomous run leva uma tarefa até uma condição de término.

Fora do pstack.

- `/deslop`, `control-cli` e `control-ui` vêm no plugin `cursor-team-kit`.
- `/loop` e `/create-skill` são nativos do Cursor.
- O pstack não tem skill `/orchestrate`. Se o menu de barra mostra `/orchestrate`, outro plugin fornece.

## Playbooks e princípios

Playbooks são listas de passos dentro do `/poteto-mode`, não skills, então não têm comando de barra. Dentro do `/poteto-mode`, descrever a tarefa escolhe um, e estas frases nomeiam um direto.

- "babysit this pr" ou "check on pr 123" roda Babysit. Ele leva o PR até pronto para merge e para ali. Não faz merge a menos que o usuário peça merge, land ou ship.
- "land the stack" roda Shipping.
- "take over this branch" roda Session pickup.
- "pause safely" roda Pause safely.
- "full autopilot on this queue" roda Autopilot-full. "autopilot, don't merge" roda Autopilot-stack.
- "run the eval playbook" roda Eval.

Sem `/poteto-mode`, uma frase como "babysit this pr" pode iniciar a skill do próprio Cursor para o mesmo trabalho. A seção Playbooks de [`poteto-mode`](../poteto-mode/SKILL.md) lista todos os playbooks. A [página 6 do guia](../../../docs/guide/06-verify-and-ship.md) cobre abrir, babysit e aterrissar um PR.

O pstack não tem skill de planejamento. O Plan Mode do Cursor trabalha junto. Para trabalho que atravessa fases ou PRs empilhados, pedir um plano ao `/poteto-mode` roda o [playbook Multi-phase plan](../poteto-mode/playbooks/multi-phase-plan.md), que escreve o plano e não implementa. Para uma questão de design, o playbook Prototype ou o `/architect` resolve no código primeiro.

Princípios são skills de uma regra que o `/poteto-mode` lê e cita nas respostas. O usuário raramente invoca um. Ele direciona pelos nomes, como em "apply prove it works. show me the real output." Digitar `/principle-<name>` ainda carrega um sob demanda. A [página 8 do guia](../../../docs/guide/08-principles.md) lista eles.

## Corrija uma execução que deu errado

| Sintoma | Correção |
|---|---|
| O modo parou de aplicar depois de alguns turnos | Ele foi iniciado com Enter. Inicie como Custom Mode, ou comece cada tarefa com `/poteto-mode`. |
| Uma pergunta foi tratada como próximo passo da última tarefa | Diga "tarefa nova", ou diga que o turno não precisa do modo. |
| Uma escolha nova de modelo não teve efeito | A regra do `/setup-pstack` vale para chats novos. Comece um. |
| As execuções custaram mais que o esperado | Veja o parágrafo de custo em Configure. |
| Uma skill não carregou sozinha | Só o `/setup-pstack` carrega a partir das palavras do usuário. As outras carregam quando o usuário digita elas ou quando o `/poteto-mode` roda elas, e ele não roda toda skill. |
| Agentes paralelos sobrescreveram um ao outro | Dê a cada agente o próprio worktree, ou rode eles como agentes de nuvem, que cada um ganha a própria máquina. |
| Uma execução da noite avançou mas não terminou nada | O `/loop` precisa de uma checagem que possa passar ou falhar, não de uma duração. Veja a [página 7 do guia](../../../docs/guide/07-overnight.md). |
| A resposta afirma sucesso por causa de um build verde | Peça o comando, fluxo, valor armazenado ou profile real. Esse é o princípio prove-it-works. |

Para uma execução que desvia, [`references/prompting.md`](references/prompting.md) tem direcionamentos de uma linha. A [página 10 do guia](../../../docs/guide/10-recipes-and-pitfalls.md) tem mais armadilhas e as receitas que valem copiar.

## Deixe o pstack com a minha cara

- [`/automate-me`](../automate-me/SKILL.md) rascunha uma skill de modo pessoal a partir do histórico do próprio usuário, para usar junto com `/poteto-mode`.
- [`/reflect`](../reflect/SKILL.md) depois de uma sessão transforma as lições dela em edições de skill que o usuário aprova.
- `/poteto-mode write a skill for <workflow>` roda o playbook de autoria. O playbook de eval testa uma mudança de skill às cegas.
- Corrija uma skill com defeito no PR dela, não dentro do trabalho de funcionalidade onde ela errou.

A [página 9 do guia](../../../docs/guide/09-make-it-yours.md) cobre cada um destes.

## Resposta

Comece pela resposta. Dê no máximo um prompt de exemplo num bloco de código, adaptado de [`references/recipes.md`](references/recipes.md) quando um servir, depois o link para esse arquivo. Seja curto a menos que o usuário tenha pedido o mapa inteiro.
