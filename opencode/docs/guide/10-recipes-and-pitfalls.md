# Receitas e armadilhas

Prompts que valem copiar, depois os erros que todo mundo comete uma vez. Troque pelos seus próprios caminhos e condições de término. As receitas são informais de propósito. É assim que elas são digitadas na prática, e as skills leem a intenção bem.

![Ela prova um prato pronto enquanto robôs cozinham a partir de uma caixa de receitas, com cartões presos lendo /how, /tdd e /loop acima do balcão.](./images/recipes.jpg)

## Entenda um subsistema desconhecido

```text
use /how first to understand how this initialization works. then use /why to figure out why it broke recently.
```

Mecânica primeiro, histórico depois. O relatório de cada skill diz quais fontes ela buscou, então você sabe no que a resposta se apoia.

## Reformule um relato confuso antes de tocar no código

```text
/poteto-mode read this thread. restate the underlying issue in your own words, in plain english. don't change any code yet.
```

Uma leitura errada aparece na reformulação, onde corrigir custa uma mensagem. Guarde sua própria teoria até o agente declarar a dele. O `/in-your-own-words` pede só a reformulação, sem playbook.

## Afie um prompt antes de enviar

```text
/sharpen write the onboarding doc for the new billing service.
```

O agente nomeia o que um especialista faria e você não pediu, a matéria-prima de onde ele partiria, os métodos e as ferramentas dele, e uma checagem externa de que o resultado está certo. Ele devolve seu prompt reescrito para pedir isso e não faz a tarefa. Envie a reescrita quando ela soar certa.

## Prototipe antes de escolher

```text
/poteto-mode prototype a few options for the settings layout. put them behind a switcher and send me screenshots of each.
```

Você escolhe entre coisas que rodam, não entre descrições. O agente responde as próprias perguntas de layout e de tempo no caminho.

## Transforme um design fechado em plano

```text
/poteto-mode turn this design into a plan. small verifiable PRs, each with its own proof.
```

Peça só depois que o design fechar. O plano é a entrega, e ele nomeia o playbook que vai executá-lo.

## Peça uma segunda opinião sobre um design

```text
ask /arena for a second opinion on this thread and our approach
```

Seu design atual vira um candidato entre vários, e a síntese diz se o painel achou algo melhor ou confirmou o que você tinha. Custa pouco perto de um design caro de reverter.

## Cheque fatias independentes em paralelo

```text
/swarm check every package under packages/ against its check.sh. one worker per package. one report.
```

Cada worker é dono de um pacote. O pai espera toda fatia e devolve um relatório único `PASS`, `ISSUES` ou `BLOCKED` em vez de despejos crus dos workers.

## Revise um branch com ceticismo

```text
/interrogate the whole branch, but skeptically. don't change anything yet. no nitpicks unless it's an actual bug or regression in behavior.
```

Os qualificadores fazem trabalho de verdade. "don't change anything yet" mantém a leitura só, e a regra de nitpick pré-filtra o ruído para os achados verificados valerem seu tempo.

## Corrija um bug por um teste que falha

```text
/poteto-mode repro the duplicate write first. if there's a cheap test path, /tdd it. then fix and rerun.
```

"if there's a cheap test path" importa. Forçar um teste por mocks frágeis prova menos do que rodar o comando real, e o playbook pode dizer isso.

## Reproduza e corrija um relato com prova

```text
/poteto-mode repro this with /verify-<app>. if it repros on main, fix it and show me a video as proof.
```

"if it repros on main" deixa a execução parar cedo quando o bug já sumiu. O vídeo deixa você checar a correção antes de ler o diff.

## Prove uma propriedade que testes raramente alcançam

```text
/create-formal-verification-skill the retry queue. a message must never be delivered twice.
```

Você ganha uma propriedade checada em todo caso dentro de um escopo declarado, um contraexemplo nos termos do seu código se o código já a quebra, e uma skill de projeto que o próximo agente roda em qualquer mudança naquele código.

## Avalie um número antes de publicar

```text
/benchmark-checklist vet this 40% speedup before it goes in the pr description
```

Você recebe mais rápido, mais lento, nenhuma diferença mensurável, ou inconclusivo, com a contagem de execuções, a faixa e o que limita o número.

## Pare de corrigir o mesmo erro

```text
/correct agents keep adding new config flags without registering them in the schema
```

A correção aterrissa no repositório como arquitetura, um tipo, um lint ou um teste, então o próximo agente não consegue cometer o erro.

## Pergunte como fazer sem começar o trabalho

```text
/poteto-help how do i get poteto-mode to stay on every turn?
```

Você ganha uma resposta, um prompt para enviar, e um link para a fonte. Nada roda até você enviar esse prompt.

## Mantenha uma execução honesta enquanto você está fora

```text
im going to bed, keep going autonomously until every fixture passes. do not stop. keep a decision log i can audit in the morning.
```

O contrato completo está na [página da noite](./07-overnight.md). A forma curta funciona quando a tarefa e a condição de término já estão na conversa.

## Redirecione uma execução à deriva

Prompts de direção são uma linha:

```text
i said the goal is to repro. i did not ask for a fix yet.
```

```text
apply prove it works. show me the real output, not the build log.
```

```text
/unslop that, no emdashes
```

Raramente você precisa de mais palavras. Precisa do nome certo, e a [página de princípios](./08-principles.md) é o vocabulário.

## Receba a resposta em palavras simples

```text
/bro
```

Esse é o prompt inteiro. O [`/bro`](../../.opencode/skills/bro/SKILL.md) reformula a última mensagem como um humano falando com outro, sem jargão, mais curto. Use quando uma resposta é tecnicamente completa e você ainda não sabe o que ela disse.

## Recupere o atraso de um chat que você deixou rodando

```text
/ketchup
```

O [`/ketchup`](../../.opencode/skills/ketchup/SKILL.md) diz em palavras simples o que mudou desde a sua última mensagem, depois lista cada chamada esperando você com a escolha dele, a evidência e o risco. Ele não muda nada enquanto relata.

Para assistir ao resumo em vez disso, rode o [`/ketchup-video`](../../.opencode/skills/ketchup-video/SKILL.md). Ele mostra o que aconteceu num vídeo curto de slides com legendas, e lista as chamadas esperando você em texto embaixo.

## As armadilhas

- **Enumerar skills no prompt.** "use /architect then /arena then /interrogate" reordena passos que o playbook já sequencia. Declare o objetivo e as restrições. Nomeie uma skill só para sobrescrever um padrão.
- **Uma condição de término vaga.** "make it better" não dá nada ao `/loop` para checar. Dê um comando ou artefato que passa ou falha.
- **Começar pela sua teoria da causa.** O agente busca onde você apontou. Peça a reformulação do problema primeiro, depois compartilhe seu palpite.
- **Ficar com o primeiro design.** Uma tentativa trava a primeira forma que o modelo pensou. Peça protótipos ou `/architect` e escolha pela evidência.
- **Polir um plano abstrato.** Revisão adversarial de um plano sem código atrás inventa riscos que nunca vão acontecer. Feche as perguntas abertas com protótipos, depois revise o que foi construído.
- **Agentes em paralelo no mesmo worktree.** Eles sobrescrevem uns aos outros, e ninguém consegue dizer qual agente mudou o quê. Rode como agentes na nuvem, ou diga "own worktree per attempt".
- **Rodar em laço antes de confiar no laço.** Um laço que não consegue verificar o próprio trabalho só acelera trabalho não checado. Faça a skill de verificação funcionar primeiro.
- **Confiar num número não avaliado.** Um cache quente ou um caminho de código pulado pode forjar um ganho de velocidade. Rode `/benchmark-checklist` antes de o número ir a qualquer lugar.
- **Corrigir o mesmo erro na mão.** Uma correção no chat ajuda uma execução. O `/correct` conserta o repositório para nenhuma execução posterior repetir.
- **Usar `/arena` para cobertura.** O `/arena` repete um briefing de design ou de código, depois escolhe uma base e enxerta as melhores partes. O `/swarm` particiona fatias ou braços de corrida declarados e agrega um relatório.
- **Aceitar todo comentário de revisão.** Bots e humanos colocam achados reais e ruído na mesma lista. O `/interrogate` mantém só os achados que uma nova execução prova e lista o que descartou e por quê, e você pode sobrescrever de qualquer lado.
- **Tratar `auto` como slug de modelo.** `auto` e `inherit-parent` significam "omita o campo model para o subagente herdar o modelo do chat pai". [Configuração](./01-setup.md) cobre os papéis.
- **Relatar sucesso com base num build verde.** Um build prova que compila. Peça o comando real, o fluxo, o valor gravado ou o profile, e espere a evidência na resposta.
- **Escrever um `SKILL.md` à mão livre.** Roteie pelo [playbook Authoring or modifying a skill](../../.opencode/skills/poteto-mode/playbooks/authoring-a-skill.md) para a validação e a revisão acontecerem.

Esse é o guia. Se você pulou adiante, volte para a [configuração](./01-setup.md) e rode uma tarefa real. Os hábitos pegam pelo uso, não pela leitura.

Volte ao [índice do guia](./README.md).
