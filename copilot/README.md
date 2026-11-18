# pstack

eu sou o [poteto](https://x.com/poteto). não sou presidente nem ceo, mas já trabalhei com milhões de linhas de código na Meta, na Netflix e na Cursor. também estou no time core do react, onde ajudo a construir e manter o react compiler.

existe uma sensação crescente de que a ia escreve slop demais. concordo. não quero entregar código como um time de vinte artistas de slop. throughput sem qualidade não é uma meta que eu persiga. se você quer ir rápido, vá fundo primeiro.

**o pstack é a minha resposta.** são as mesmas skills que eu uso todo dia para entregar código de alta qualidade na Cursor. isso transforma o cursor num time de engenharia de verdade. o objetivo não é maximizar loc, na verdade é o oposto. o pstack te ajuda a escrever menos, e com mais qualidade.

**o pstack te dá paralelismo sem medo.** quando você consegue ir fundo em um agente e confiar que ele escreve código bom e verificável, aí sim dá para paralelizar com confiança. suba vários agentes com o `poteto-mode` e confie que eles vão aplicar princípios rigorosos de engenharia no trabalho.

**o cursor te dá o melhor de todos os mundos.** todo modelo de fronteira tem forças e fraquezas. use qualquer modelo com o pstack. aliás, muitas das minhas skills usam fluxos com vários modelos para aproveitar o que cada um tem de melhor.

faça um fork. melhore. deixe com a sua cara. PRs são bem-vindos!

## instalar

copie `.github/` e `docs/` para a raiz do repositório e commite. o github copilot descobre skills, agentes e prompts sozinho.

`/poteto-mode` vem do prompt file em `.github/prompts/`. as outras skills carregam por relevância. o [mapa deste harness](./docs/harness/copilot.md) mostra o que vira o quê, e aqui o bugbot e os agentes na nuvem do cursor têm equivalente nativo.

## começar

dois passos:

1. rode o [`/setup-pstack`](./skills/setup-pstack/SKILL.md), defina um orçamento de raciocínio e escolha quais modelos você quer.
2. use o [`/poteto-mode`](./skills/poteto-mode/SKILL.md) sempre que estiver fazendo algo que exige rigor.

novo por aqui? o [guia do pstack](./docs/guide/README.md) te leva por uma primeira tarefa de verdade, do setup e do prompt até a verificação e execuções da noite para o dia. travado, ou sem saber qual skill serve? pergunte ao [`/poteto-help`](./skills/poteto-help/SKILL.md).

é isso. as outras skills são situacionais, e a skill de modo usa elas por você quando precisa. de fábrica, delegados de código (feature, refactoring, bug fix, perf, hillclimb), prosa e julgamento vão para o opus 5.5 no xhigh, e nenhum papel roda acima de xhigh. fora de um painel de revisão, o fable 5.2 roda só quando você o nomeia ou depois de duas execuções do opus 5.5 no mesmo briefing falharem. o painel padrão é fable 5.2 / opus 5.5 / gemini 4 carbon. o [`/setup-pstack`](./skills/setup-pstack/SKILL.md) muda qualquer coisa disso.

## uso

use o [`/poteto-mode`](./skills/poteto-mode/SKILL.md) no começo de uma tarefa. ele lê o seu pedido, escolhe num conjunto de playbooks e roda as outras skills conforme os passos precisam.

escolha o `/poteto-mode` no menu `/` antes de digitar a sua tarefa, para o cursor anexar a skill à mensagem.

### só use o [`/poteto-mode`](./skills/poteto-mode/SKILL.md)

esta skill é o atalho principal. uso ela sempre que preciso que o agente faça trabalho de engenharia rigoroso. ela vem com vinte e dois playbooks:

```
/poteto-mode this pr has a subtle bug where the scroll drifts every 750ms even when idle. repro
first, then fix and verify.
```

```
/poteto-mode i'm going to bed. land the stack even if ci flakes. i want everything merged by
morning.
```

<details>
<summary>os vinte e dois playbooks</summary>

| playbook | para |
|---|---|
| [investigation](./skills/poteto-mode/playbooks/investigation.md) | uma pergunta só de leitura. como x funciona, por que y foi construído assim, temos certeza. |
| [bug fix](./skills/poteto-mode/playbooks/bug-fix.md) | reproduza um defeito, ache a causa raiz e corrija com evidência de runtime. |
| [perf](./skills/poteto-mode/playbooks/perf-issue.md) | rastreie uma lentidão medida e melhore contra uma linha de base. |
| [hillclimb](./skills/poteto-mode/playbooks/hillclimb.md) | melhoria sustentada e científica de uma métrica contra um alvo, em laço de hipóteses com medição antes e depois e um commit por ganho aceito. |
| [runtime forensics](./skills/poteto-mode/playbooks/runtime-forensics.md) | diagnostique um sintoma ao vivo (vazamento, cpu girando em ociosidade, falha visual) a partir de instrumentação. |
| [trace forensics](./skills/poteto-mode/playbooks/trace-forensics.md) | diagnostique um artefato de profiling capturado (cpuprofile, trace, spindump, heap snapshot). |
| [feature](./skills/poteto-mode/playbooks/feature.md) | comportamento novo ou alterado, construído a partir de uma forma de dados nomeada. |
| [refactoring](./skills/poteto-mode/playbooks/refactoring.md) | uma mudança que preserva comportamento, em estrutura ou forma. |
| [prototype](./skills/poteto-mode/playbooks/prototype.md) | um esboço descartável para decidir design ou comportamento barato, ou para resolver uma bifurcação empírica observando ela. |
| [visual parity](./skills/poteto-mode/playbooks/visual-parity.md) | equivalência de ui exata ao pixel entre duas implementações. |
| [authoring a skill](./skills/poteto-mode/playbooks/authoring-a-skill.md) | escrever ou editar um SKILL.md. |
| [eval](./skills/poteto-mode/playbooks/eval.md) | teste como uma mudança de skill ou de prompt afeta o comportamento do agente, às cegas. |
| [babysit](./skills/poteto-mode/playbooks/babysit.md) | leve um pr ou uma pilha até pronto para merge, cuidando de conflitos, threads de revisão e ci. |
| [shipping](./skills/poteto-mode/playbooks/shipping.md) | verifique uma pilha verde de forma independente, depois aterrisse a sequência contígua verificada de baixo para cima pelo github por padrão, ou pelo origin quando existir. |
| [autonomous run](./skills/poteto-mode/playbooks/autonomous-run.md) | leve uma tarefa longa até o fim sem parar. |
| [autopilot-full](./skills/poteto-mode/playbooks/autopilot-full.md) | leve prs independentes até o merge com um dono por pr. um swarm verifica cada cabeça pronta em código, e toda mudança posterior no patch ganha uma checagem nova. |
| [autopilot-stack](./skills/poteto-mode/playbooks/autopilot-stack.md) | o autopilot-full sem o merge. constrói e verifica prs para o operador revisar e aterrissar, empilhando um pr só em cima de um pr em que ele se baseia. |
| [session pickup](./skills/poteto-mode/playbooks/session-pickup.md) | retome ou assuma o trabalho em andamento de um agente anterior. |
| [pause safely](./skills/poteto-mode/playbooks/pause-safely.md) | suspenda trabalho em andamento de forma limpa para retomar depois. |
| [multi-phase plan](./skills/poteto-mode/playbooks/multi-phase-plan.md) | trabalho que atravessa fases ou PRs empilhados. |
| [worktree cleanup](./skills/poteto-mode/playbooks/worktree-cleanup.md) | recupere disco podando worktrees já mergeados ou abandonados e simuladores ios obsoletos, com trava de segurança. |
| [opening a pr](./skills/poteto-mode/playbooks/opening-a-pr.md) | abra um pr pronto a partir de commits pequenos e ordenados, com título em conventional commits e corpo em estilo de briefing. invocado no fim de todo outro playbook. |

</details>



quando invocado, ele:

1. casa a sua tarefa com um [playbook](./skills/poteto-mode/playbooks/) e abre uma lista de tarefas cujos primeiros itens são os passos dele, copiados na íntegra.
2. roteia para as outras skills conforme os passos disparam.
3. escreve respostas sem slop, enquadradas para quem consome e para quem mantém.

as regras e os playbooks completos ficam em [`skills/poteto-mode/SKILL.md`](./skills/poteto-mode/SKILL.md).

para manter o [`/poteto-mode`](./skills/poteto-mode/SKILL.md) ligado entre turnos, escolha ele no menu `/` e aperte option+enter (mac) ou alt+enter (windows) em vez de enter. isso vira um [modo customizado](https://cursor.com/docs/skills#using-a-skill-as-a-custom-mode), que o cursor oferece na janela de agentes e na cli. ele fica no contexto a cada turno até você sair do modo. enter puro anexa só a uma mensagem.

o [`/poteto-mode`](./skills/poteto-mode/SKILL.md) funciona muito bem com o comando `/loop` do cursor. dá para fazer o cursor trabalhar por muitas horas sem abrir mão do rigor.

## skills

o [`/poteto-mode`](./skills/poteto-mode/SKILL.md) roda a maioria destas por você quando um passo precisa (`why`, `architect`, `arena`, `swarm`, `interrogate`, `unslop`, `no-comments`, `technical-writing`, `tdd`, e os princípios). a tabela abaixo é para quando você quer uma direto:

```
/how do we cancel runs? do we have an n+1 when we look up every run to cancel?
```

```
/interrogate review this pr.
```

<details>
<summary>todas as skills</summary>

| skill | use quando |
|---|---|
| [`/poteto-mode`](./skills/poteto-mode/SKILL.md) | ponto de entrada padrão para qualquer tarefa não trivial. |
| [`/poteto-help`](./skills/poteto-help/SKILL.md) | você é novo no pstack, ou não sabe qual skill, playbook ou princípio serve. descobre o que você está tentando fazer, responde essa parte e te entrega um prompt para digitar. roda só quando você digita `/poteto-help`. |
| [`/in-your-own-words`](./skills/in-your-own-words/SKILL.md) | você quer que o agente devolva o seu objetivo e o problema antes de começar. roda só quando você digita `/in-your-own-words`. |
| [`/sharpen`](./skills/sharpen/SKILL.md) | você quer um prompt mais fundo, não maior, antes de mandar. o que um profissional de ponta faria e você não pediu, a matéria-prima de onde ele partiria, os métodos e as ferramentas dele, e uma checagem externa de que o resultado está certo. devolve o prompt reescrito, que pede ao agente para prototipar toda escolha que um experimento resolve e deixa as escolhas de produto e de preferência com você. não faz a tarefa. roda só quando você digita `/sharpen`. |
| [`/how`](./skills/how/SKILL.md) | você quer um passeio por como um subsistema funciona. |
| [`/why`](./skills/why/SKILL.md) | você quer saber por que algo foi construído assim. responde a partir do registro, não do código. os commits e as discussões de pr que introduziram aquilo e cada mudança posterior, e outras fontes quando essa trilha é fraca. cita o que os autores escreveram, marca as próprias inferências e diz o que o registro não responde. |
| [`/recall`](./skills/recall/SKILL.md) | você está começando ou retomando trabalho e quer o seu contexto recente sobre um assunto reconstruído a partir do seu próprio histórico de chat e do registro compartilhado, devolvido como um briefing enxuto do estado atual. |
| [`/ketchup`](./skills/ketchup/SKILL.md) | você quer o que mudou neste chat desde a sua última mensagem, em palavras simples e visuais pequenos, com todo item que espera por você listado e decidível só pela resposta, mesmo que isso deixe a resposta longa. |
| [`/ketchup-video`](./skills/ketchup-video/SKILL.md) | você quer o resumo do `/ketchup` como um vídeo curto de slides com legenda, com as decisões que esperam por você como texto embaixo. |
| [`/blast-radius`](./skills/blast-radius/SKILL.md) | você tem uma mudança de aparência pequena e quer saber o que mais ela pode quebrar, com o fato que a torna segura provado rodando código, não afirmado. |
| [`/architect`](./skills/architect/SKILL.md) | você está prestes a escrever código que atravessa uma fronteira de função e quer o uso do chamador, os tipos e a forma do módulo resolvidos antes. |
| [`/arena`](./skills/arena/SKILL.md) | você quer N tentativas em paralelo para a mesma coisa, e depois pegar as melhores partes de cada uma. |
| [`/swarm`](./skills/swarm/SKILL.md) | você quer N workers em paralelo em fatias ou corridas diferentes, e depois um relatório agregado. |
| [`/interrogate`](./skills/interrogate/SKILL.md) | você tem um diff e quer que modelos diferentes tentem quebrar ele, guardando só achados que uma execução prova. |
| [`/automate-me`](./skills/automate-me/SKILL.md) | você quer a sua própria skill `-mode`, rascunhada a partir de como você realmente trabalhou. |
| [`/make-bot-ui`](./skills/make-bot-ui/SKILL.md) | você quer uma página ou dashboard cujos botões acordam um Grok Bot por webhook, incluindo a passagem da chave do remetente e o Tailscale. |
| [`/setup-pstack`](./skills/setup-pstack/SKILL.md) | você quer escolher quais modelos o pstack usa por papel. detecta os seus modelos e escreve uma regra de configuração. |
| [`/reflect`](./skills/reflect/SKILL.md) | uma tarefa longa aterrissou e você quer a receita capturada como edição de skill. |
| [`/correct`](./skills/correct/SKILL.md) | você vive corrigindo agentes pelos mesmos erros. minera o histórico atrás de classes de erro, corrige cada uma no nível mais alto que funciona (arquitetura, depois tipos, lint e ci, depois testes, com docs por último), e mantém uma tabela que pareia cada regra com o que a aplica. |
| [`/teach`](./skills/teach/SKILL.md) | você quer entender de fato uma mudança ou um subsistema, não só receber um resumo. roda how + why e tece uma explicação simples, construída diagrama por diagrama. |
| [`/tdd`](./skills/tdd/SKILL.md) | você está corrigindo um bug e existe um caminho de teste local barato. escreva o teste que falha primeiro, depois a correção. |
| [`/benchmark-checklist`](./skills/benchmark-checklist/SKILL.md) | você rodou um benchmark ou mediu um ganho de velocidade ou uma regressão. audita o número (limitador, ajuste, erros, execuções repetidas, relevância fim a fim) antes de você relatar ou agir sobre ele. |
| [`/no-comments`](./skills/no-comments/SKILL.md) | tire os comentários antes da revisão. cria o Comment Sicko, corrige os achados aceitos, oferece codificações para restrições alegadas. |
| [`/typescript-best-practices`](./skills/typescript-best-practices/SKILL.md) | você está lendo ou editando typescript. aterra o princípio de disciplina de sistema de tipos na sintaxe. |
| [`/figure-it-out`](./skills/figure-it-out/SKILL.md) | nenhum playbook empacotado serve. projeta um playbook rigoroso e auditável para a tarefa. |
| [`/show-me-your-work`](./skills/show-me-your-work/SKILL.md) | você quer uma trilha de decisões revisável. registra decisões num tsv que você pode commitar. |
| [`/create-verification-skill`](./skills/create-verification-skill/SKILL.md) | o seu projeto não tem jeito roteirizado de provar o comportamento do app. gera uma skill de verificação local do projeto com um mapa de funcionalidades, para qualquer linguagem ou plataforma. |
| [`/maintain-verification-skill`](./skills/maintain-verification-skill/SKILL.md) | o mapa de funcionalidades da sua skill de verificação divergiu do app. onda no código-fonte mais uma passada ao vivo, no máximo um PR de correções provadas. |
| [`/create-formal-verification-skill`](./skills/create-formal-verification-skill/SKILL.md) | os testes não alcançam o caso que quebra uma corrida, uma retentativa, uma máquina de estados, um parser ou código unsafe. verifica uma propriedade com a ferramenta que serve e gera uma skill local do projeto para rechecar ela a cada mudança. |
| [`/maintain-formal-verification-skill`](./skills/maintain-formal-verification-skill/SKILL.md) | o código que a sua verificação formal cobre mudou desde a última checagem. atualiza o modelo, o harness ou as propriedades para casar com o código, roda de novo e diz se cada falha é deriva da verificação ou bug no código. |
| [`/statechart`](./skills/statechart/SKILL.md) | alguém que é dono do comportamento precisa ver ele, não ler uma spec. checa um modelo tla+, desenha o que o usuário vê como um statechart mermaid, marca cada estado que quebra uma regra e dá o caminho mais curto até cada um. |
| [`/unslop`](./skills/unslop/SKILL.md) | você está limpando texto. remove marcas de ia. |
| [`/bro`](./skills/bro/SKILL.md) | você quer a última mensagem recontada em linguagem humana simples, sem jargão. |
| [`/technical-writing`](./skills/technical-writing/SKILL.md) | padrão de doc em camadas (Diátaxis + Google developer style + STE + Global English) para docs, RFCs, readmes, descrições de PR, mensagens de commit. |

</details>



### exemplos

na maior parte do tempo eu digito o [`/poteto-mode`](./skills/poteto-mode/SKILL.md) no começo de uma tarefa e deixo ele rotear para um playbook. as outras skills disparam conforme os passos precisam. algumas eu pego direto.


<details>
<summary>todos os exemplos</summary>

```
bug fix:           /poteto-mode this pr has a subtle bug where the scroll drifts every 750ms even
                   when idle. repro first, then fix and verify.
perf:              /poteto-mode a big list takes a second or two to load even though we virtualize.
                   run a cpu trace and tell me why.
feature:           /poteto-mode build a small feature behind a feature flag. verify it really works.
prototype:         /poteto-mode build two prototypes of the markdown renderer so we can compare.
                   spawn an agent for each.
multi-phase:       /poteto-mode open source these skills as a plugin. nothing internal leaks, work
                   in a temp dir, show me the dependency graph first.
overnight run:     /poteto-mode i'm going to bed. land the stack even if ci flakes. i want
                   everything merged by morning.
babysit:           /poteto-mode check on pr 123. anything outstanding?
visual parity:     /poteto-mode the row spacing is too tall when this flag is on. the second image
                   is correct. repro and fix until it matches.
figure it out:     /poteto-mode i'm stepping away. migrate every caller from the synchronous store
                   to the new async one, keeping behavior identical. i want to trust it was done
                   right when i'm back.
how:               /how do we cancel runs? do we have an n+1 when we look up every run to cancel?
why:               /why is this feature flag not on yet?
architect:         design this instrumentation to be high signal with no false positives. /architect
                   this first.
arena:             /arena take my prompt to the arena verbatim. i want to compare their proposals
                   with yours.
swarm:             /swarm check every package under packages/ against its check.sh. one worker per
                   package. one report.
interrogate:       /interrogate review this pr.
tdd:               /tdd implement
unslop:            can we unslop and tighten the new changes?
reflect:           /reflect that took too long. capture what we learned so the next run doesn't
                   repeat it.
correct:           /correct
show-me-your-work: /show-me-your-work keep a decision trail i can review when i'm back.
automate-me:       /automate-me
help:              /poteto-help which skill should i use to review this branch?
```

</details>

## os subagentes `poteto-agent` e Comment Sicko

o [`/poteto-mode`](./skills/poteto-mode/SKILL.md) roda no chat que o recebe. esse chat é dono do playbook, da lista de tarefas e de todo spawn de subagente.

o pstack também entrega o [`subagent_type: "poteto-agent"`](./agents/poteto-agent.md) como o wrapper para delegados criados durante uma execução. ele lê o `poteto-mode` inteiro, incluindo o índice de princípios embutido, antes de fazer qualquer trabalho. trocar por `generalPurpose` pula essa leitura e deriva. não crie um `poteto-agent` para rodar o playbook em si.

o [`/poteto-mode`](./skills/poteto-mode/SKILL.md) e o [`subagent_type: "poteto-agent"`](./agents/poteto-agent.md) passam pelo mesmo wrapper.

o pstack também entrega o [Comment Sicko](./agents/comment-sicko.md), um revisor de comentários só de leitura disponível como `subagent_type: "comment-sicko"`. normalmente invoque ele pelo [`/no-comments`](./skills/no-comments/SKILL.md), não direto.

## princípios

vinte e quatro skills curtas, um princípio cada. o `poteto-mode` indexa elas embutidas e lê esse índice no começo da tarefa. os arquivos soltos existem para outras skills referenciarem um princípio pelo nome, e para o índice apontar a regra completa de cada um.

<details>
<summary>todos os vinte e quatro princípios</summary>

| princípio | grupo | regra |
|---|---|---|
| [laziness-protocol](./skills/principle-laziness-protocol/SKILL.md) | núcleo | Penda para a deleção e para a menor mudança que resolve o problema. |
| [foundational-thinking](./skills/principle-foundational-thinking/SKILL.md) | núcleo | Aplique antes de escrever lógica: escolher tipos e estruturas de dados centrais, sequenciar trabalho de andaime contra funcionalidade, perguntar o que atores concorrentes compartilham. Acerte as estruturas de dados para o código a jusante ficar óbvio. |
| [redesign-from-first-principles](./skills/principle-redesign-from-first-principles/SKILL.md) | núcleo | Redesenhe como se o requisito fosse uma premissa fundacional desde o primeiro dia, em vez de parafusar ele depois. |
| [attack-the-premise](./skills/principle-attack-the-premise/SKILL.md) | núcleo | Aplique quando duas ou mais correções que compartilham uma premissa falharam no mesmo portão. Escreva a premissa e teste ela direto antes de escrever outra correção que a assume. |
| [subtract-before-you-add](./skills/principle-subtract-before-you-add/SKILL.md) | núcleo | Remova peso morto, validadores redundantes e referências a stub primeiro, depois construa sobre a base mais simples. |
| [minimize-reader-load](./skills/principle-minimize-reader-load/SKILL.md) | núcleo | Conte camadas entre a pergunta e a resposta, e estado escondido na cabeça do leitor. Colapse wrappers de um único chamador e encolha o escopo mutável. |
| [outcome-oriented-execution](./skills/principle-outcome-oriented-execution/SKILL.md) | núcleo | Aplique durante reescritas e migrações planejadas com fronteiras de fase explícitas. Convirja para a arquitetura alvo. Não preserve estados intermediários suaves com código de compatibilidade descartável. |
| [experience-first](./skills/principle-experience-first/SKILL.md) | núcleo | Escolha o encanto do usuário acima da conveniência de implementação. Entregue menos funcionalidades bem acabadas em vez de mais funcionalidades rascunhadas. |
| [exhaust-the-design-space](./skills/principle-exhaust-the-design-space/SKILL.md) | núcleo | Construa 2 ou 3 protótipos concorrentes e compare lado a lado antes de se comprometer. |
| [build-the-lever](./skills/principle-build-the-lever/SKILL.md) | núcleo | Aplique a qualquer trabalho não trivial, não só trabalho em volume: edições, migrações, análises, checagens. Construa a ferramenta que faz isso ou prova isso (codemod, script, gerador, ou uma skill que os seus subagentes seguem) em vez de trabalhar na mão. A ferramenta é o artefato que um revisor pode rodar de novo. |
| [model-the-domain](./skills/principle-model-the-domain/SKILL.md) | arquitetura | Codifique o domínio numa estrutura em vez de condicionais espalhados. |
| [boundary-discipline](./skills/principle-boundary-discipline/SKILL.md) | arquitetura | Concentre guardas nas fronteiras do sistema (CLI, config, rede, APIs externas). Confie nos tipos internos e mantenha a lógica de negócio em funções puras. |
| [type-system-discipline](./skills/principle-type-system-discipline/SKILL.md) | arquitetura | Torne estados ilegais irrepresentáveis, marque primitivos semânticos, faça parse de dados externos nas fronteiras, esgote as variantes, derive de schemas autoritativos. |
| [make-operations-idempotent](./skills/principle-make-operations-idempotent/SKILL.md) | arquitetura | Convirja para o mesmo estado final independente de execuções parciais anteriores. |
| [migrate-callers-then-delete-legacy-apis](./skills/principle-migrate-callers-then-delete-legacy-apis/SKILL.md) | arquitetura | Migre os chamadores e apague a API antiga na mesma onda, em vez de preservar camadas de compatibilidade. |
| [separate-before-serializing-shared-state](./skills/principle-separate-before-serializing-shared-state/SKILL.md) | arquitetura | Elimine o compartilhamento primeiro. Serialize estruturalmente só quando um escritor compartilhado é um invariante real. |
| [prove-it-works](./skills/principle-prove-it-works/SKILL.md) | verificação | Aplique depois de concluir uma tarefa, antes de declarar pronto. Verifique contra o artefato real (rode a funcionalidade, leia o valor de verdade, inspecione o diff), não contra um proxy, um autorrelato ou um "compila". |
| [fix-root-causes](./skills/principle-fix-root-causes/SKILL.md) | verificação | Rastreie cada sintoma até o primeiro ponto onde o sistema errou e corrija ali. Uma guarda só é correção quando o estado que ela trata é legítimo. |
| [sequence-verifiable-units](./skills/principle-sequence-verifiable-units/SKILL.md) | verificação | Aplique a trabalho de várias etapas (varreduras, migrações, sequências de edições parecidas) e a como você empilha commits e PRs. Quebre o trabalho em unidades pequenas que terminam cada uma num estado verificável, checando cada uma antes da próxima, e ordene a entrega para que a sequência se prove para um revisor. |
| [test-behavior-not-implementation](./skills/principle-test-behavior-not-implementation/SKILL.md) | verificação | Aplique quando você escreve, muda ou mantém um teste. Afirme o que os usuários do código podem observar, para o teste falhar quando esse comportamento quebra e continuar passando quando só o interno muda. |
| [explain-the-number](./skills/principle-explain-the-number/SKILL.md) | verificação | Aplique antes de confiar, relatar ou agir sobre um número que você mediu: um ganho de velocidade, uma regressão, um throughput, uma latência ou um resultado de eval. Descubra o que o limita e descarte que ele mediu algo diferente do trabalho que você pensa. |
| [guard-the-context-window](./skills/principle-guard-the-context-window/SKILL.md) | delegação | Roteie o volume para subagentes. Mantenha resumos na thread principal, não payloads crus. |
| [never-block-on-the-human](./skills/principle-never-block-on-the-human/SKILL.md) | delegação | Siga, apresente o resultado, deixe o humano corrigir o rumo depois do fato. Reserve confirmação para ações irreversíveis. |
| [encode-lessons-in-structure](./skills/principle-encode-lessons-in-structure/SKILL.md) | meta | Codifique a regra como lint, flag de metadado, checagem de runtime ou script em vez de mais texto. |

</details>

## o que não vem aqui

algumas coisas que o `poteto-mode` referencia mas não empacota:

- `/deslop` e a skill `deslop` vêm no plugin `cursor-team-kit`.
- `control-cli` (para CLIs e TUIs) e `control-ui` (para browser, Electron, web) também vêm no `cursor-team-kit`.
- `/create-skill` é nativo do cursor. o cursor também traz um `/autopilot` nativo que observa um pr. dentro do `poteto-mode`, pedidos de status de pr vão para o [playbook babysit](./skills/poteto-mode/playbooks/babysit.md) em vez disso.

instale o `cursor-team-kit` junto com o pstack se você quer o conjunto completo.

## por que não existem skills de planejamento?

o cursor já tem um plan mode ótimo, que funciona muito bem com o pstack. mas, pessoalmente, eu não acredito em planejamento. a melhor spec é o código. se você quiser fazer um plano, o [`/poteto-mode`](./skills/poteto-mode/SKILL.md) cobre isso, mas não é o padrão.

## faça do seu jeito

o `poteto-mode` é o meu estilo. talvez você não queira exatamente isso.

digite o [`/automate-me`](./skills/automate-me/SKILL.md). ele minera as suas transcrições recentes, rascunha uma skill `<seu-nome>-mode` a partir de como você realmente trabalhou, e roteia pelo pstack por baixo. você mantém o pstack como base e termina com a sua própria skill de roteamento ao lado do `poteto-mode`.

os modelos também são configuráveis. digite o [`/setup-pstack`](./skills/setup-pstack/SKILL.md). ele detecta os modelos que você tem acesso e escreve uma regra pequena, sempre aplicada, que mapeia cada papel (código, julgamento, os painéis de revisão) para um modelo. toda skill lê essa regra e cai em padrões razoáveis quando ela não existe, então você sobrepõe só o que quiser.

quando um padrão muda, uma regra escrita antes da mudança ainda fixa o padrão antigo até você rodar o `/setup-pstack` de novo. essa execução substitui os padrões antigos que ela reconhece e mantém qualquer outro papel cujo modelo difere do padrão. para soltar um pin que ela não reconhece, apague a linha daquele papel ou o arquivo primeiro.

## automações

o pstack também entrega um [pacote de automações benny](./automations/benny/) dormente. o benny tria relatos de problema no slack, depois reproduz e corrige bugs confirmados com evidência real de ui. os arquivos dele não são registrados como slash skills.

para configurar, aponte o cursor para o [`FOR_AGENTS.md`](./automations/benny/FOR_AGENTS.md). o setup copia o pacote para o repositório alvo em `.cursor/automations/benny/`, habilita o pstack lá para skills compartilhadas, e mantém a configuração do usuário fora do pacote copiado. o benny roda nas automações do cursor, então é exclusivo do cursor.

## licença

MIT
