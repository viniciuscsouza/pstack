# Entenda o código antes de mudar ele

Editar código que você não entende é como regressões sutis são entregues, e isso vale para o agente tanto quanto para você. Agentes costumam falhar de duas formas. Eles leem errado o que você quer, ou não têm o contexto para fazer o trabalho direito. [O que entra num prompt](./02-poteto-mode.md#what-goes-in-a-prompt) trata a primeira. Esta página trata a segunda.

O pstack dá quatro entradas. `/how` explica o que o código faz agora. `/why` cava os motivos de ele ter essa forma. `/teach` mistura os dois numa explicação só. `/recall` reconstrói o seu próprio contexto recente sobre um assunto. Cada uma também obriga o agente a se explicar em palavras que você pode checar. É assim que você supervisiona um agente que talvez conheça o código melhor que você.

![Uma detetive estuda a planta de uma máquina com uma lupa enquanto robôs buscam arquivos de caso, o quadro de evidências atrás dela liga pistas sob /how e /why.](./images/understanding.jpg)

## Comece com uma investigação somente leitura

Quando a causa não está clara, peça achados, não uma correção:

```text
/poteto-mode investigate why background jobs time out every few hours. give me what we know, what data you used, and your best hypotheses. don't change any code yet.
```

"don't change any code yet" roteia isso para o [Investigation playbook](../../skills/poteto-mode/playbooks/investigation.md). Ele rastreia o código, acrescenta `/why` para perguntas sobre motivação, e devolve uma explicação citada. Para uma escolha entre opções, devolve uma recomendação com uma tabela de trade-offs. Pedir "what data you used" faz o agente separar a evidência dos palpites. Quando os achados apontam uma correção, comece a correção como tarefa nova.

## Rastreie comportamento com `/how`

```text
/how do we dedupe notifications? is there an n+1 when we look up subscribers?
```

Faça a pergunta que você de fato tem. [`/how`](../../skills/how/SKILL.md) lê o código e responde no nível de um engenheiro sênior te ambientando no subsistema, com o fluxo de runtime, os tipos-chave e as partes nada óbvias. Ele inicia um subagente somente leitura que explora o código e escreve a explicação.

## Cave o histórico com `/why`

```text
/why was the retry limit set to five? does the reason still hold?
```

[`/why`](../../skills/why/SKILL.md) responde a partir do registro, não do código. Ele lê os commits e as discussões de PR que introduziram o código e cada mudança posterior, e olha outras fontes quando essa trilha é fina. A resposta cita o que os autores escreveram, marca as inferências do próprio agente como inferências, e diz o que o registro não responde, porque "nobody wrote down why" é uma resposta em si.

As duas se compõem naturalmente. `do why first then how` é um bom prompt quando você suspeita que o histórico explica a bagunça.

## Entenda de verdade com `/teach`

```text
/teach me how this PR changes retries. convince me it fixes the cause and not the symptom.
```

[`/teach`](../../skills/teach/SKILL.md) serve para quando um resumo não basta. Ele roda `/how` e `/why`, para uma mudança pequena talvez só um deles, e tece os achados numa explicação simples que se monta diagrama por diagrama. O enquadramento "convince me" vale reusar. Ele transforma a explicação num argumento que você pode testar em vez de um passeio.

Ele funciona nas escolhas do próprio agente também:

```text
/teach me why you implemented it this way and not with a queue. what did you trade off, and why?
```

Ensinar ajuda o agente tanto quanto você. Um agente que precisa explicar seu trabalho tem que ler o código e sustentar cada afirmação com evidência, em vez de declarar com confiança e seguir.

## Reconstrua seu próprio contexto com `/recall`

```text
/recall catch me up on the export work from last week
```

[`/recall`](../../skills/recall/SKILL.md) minera seus próprios chats recentes mais o registro compartilhado (issues, correções anteriores, erros ainda disparando) e devolve um briefing de onde as coisas estão e o que vem agora. Seus chats antigos guardam contexto que um agente novo não tem, então comece trabalho novo num assunto antigo carregando ele primeiro, depois entregue a entrada nova:

```text
/recall my work on the virtualized list from yesterday, then read this bug report.
```

Se você quer retomar um chat específico, isso é o Session pickup playbook abaixo, não `/recall`.

## Assuma trabalho anterior com Session pickup

Quando outro agente (ou você, semana passada) deixou um branch no meio do voo:

```text
/poteto-mode take over this branch. read the decision log, figure out what's done, and continue from there. don't redo finished work.
```

O [Session pickup playbook](../../skills/poteto-mode/playbooks/session-pickup.md) trata a trilha anterior como autoritativa. Ele reconstrói o estado do branch e as decisões, nomeia o ponto de retomada, e verifica as afirmações herdadas contra o objetivo original em vez de rederivar tudo do zero.

**Armadilha:** não pule as skills desta página porque "the agent will read the code anyway". Um agente que começa a editar sem um modelo rastreado tende a corrigir o sintoma no primeiro ponto plausível. `/how` primeiro sai mais barato que o segundo bug.

Próximo: [Desenhe a mudança](./04-design.md).
