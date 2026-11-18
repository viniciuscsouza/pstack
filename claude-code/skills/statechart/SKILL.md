---
name: statechart
description: "Confira um modelo TLA+ de um sistema que se move entre estados, como um fluxo de produto, um protocolo ou um serviço, e desenhe ele como um statechart Mermaid. O relatório dá o caminho mais curto até cada estado que quebra uma regra. Use para /statechart, ou para mostrar um bug como um caminho por estados."
disable-model-invocation: true
---

# Statechart

O gráfico mostra como um sistema se move entre estados, nas palavras de quem o lê. Essa pessoa, o leitor, decide se o comportamento está certo.

## Rodar

Trabalhe numa pasta scratch fora do repo. Escreva uma spec TLA+ e a config dela. Depois rode:

```sh
<skill dir>/scripts/statechart SPEC.tla [--config FILE] [--view FILE] [--out DIR]
```

`scripts/statechart.ts` é TypeScript, então rode com um runtime que execute TypeScript.

A primeira execução instala TLC e Java com mise e precisa de rede.

## Escreva a spec

- Escreva como INVARIANT cada regra com que o leitor concorda. Uma regra pode ser sobre uma tela, um resultado, um valor armazenado ou uma mensagem. Se o modelo não tem variável para uma regra, adicione a variável. Nunca enfraqueça uma regra para que ela valha.
- Escreva `Next` como disjunção das ações, tipo `YouPressPlay \/ YouPressStop`. Se não fizer isso, o TLC nomeia todo passo como `Next`.
- Um estado sem passo de saída quebra `NoDeadlock`, inclusive quando é um estado final correto. Quando um INVARIANT ou um PROPERTY já tem esse nome, a regra é `CHECK_DEADLOCK`. Adicione a `Next` uma ação verdadeira só nos estados finais que não muda nada, tipo `Done == phase \in {"paid", "failed"} /\ UNCHANGED vars`, e dê a ela um rótulo em `step()`.

## Escolha o gráfico

Você faz o trabalho que exige julgamento. As palavras em cada estado e seta, as fases e as cores delas, o título, e o relatório para o leitor. Ponha seu trabalho num módulo de view, e passe ele com `--view`.

Para mostrar as palavras do leitor, a view exporta `state(vars)`. O valor que `state()` retorna escolhe a forma:

- Uma string dá um gráfico plano. `export const state = (v) => (v.paid ? "Paid" : "Paying");`
- Um array dá estados pai. `export const state = (v) => v.main.split(".");`
- Um objeto dá uma região paralela para cada chave. `export const state = (v) => ({ Playback: v.playback, Volume: v.volume });`

Rotule cada estado com o que o leitor vê ou se importa, tipo "Paying" ou "Lease held, write pending". Deixe nomes de variável fora dos rótulos. Em `vars`, uma sequência TLA+ é um objeto com chaves a partir de 1, então `v.pc[1]` é `pc[1]`. Um conjunto é um array.

A view também pode exportar `step(action, from, to)`. Ela recebe o nome da ação do TLC e os valores antes e depois do passo, e retorna o rótulo da seta. Escreva cada rótulo de passo como o usuário vê acontecer, tipo "você aperta play" ou "a conexão falha". Não nomeie o código nem a ação TLA+. Duas ações que terminam no mesmo ponto do gráfico recebem o mesmo rótulo, para dividirem uma seta.

Quando um passo só pode ocorrer em algumas condições, termine o rótulo com a condição entre colchetes, nas palavras do leitor, tipo `you press play [while the file loads]`. Num gráfico de regiões, o comando escreve a condição sobre as outras regiões na seta, nas palavras dos rótulos delas, tipo `[when Offline]`. Seus colchetes substituem isso, e duas setas saindo de um estado podem então parecer iguais. Então num gráfico de regiões, não escreva colchetes para uma condição sobre outra região. Quando um valor que nenhum rótulo mostra decide para onde um passo vai, nomeie o resultado no rótulo, tipo "a leitura da lista falha".

A view também pode exportar isto:

- `group(vars)` retorna o nome de uma fase. Cada estado do gráfico recebe a cor do grupo dele. Quando os estados do modelo por trás de um estado do gráfico estão em grupos diferentes, esse estado não recebe cor.
- `colors` dá uma classe de cor para cada nome de grupo.
- `title` é uma string que o gráfico mostra no topo.

O comando chama `state()` e `group()` no máximo uma vez por estado do modelo, e `step()` no máximo uma vez por passo. O gráfico e o relatório usam essas respostas. Então faça cada função depender só dos argumentos dela.

O gráfico não desenha um passo que fica num mesmo rótulo. Divida um rótulo quando o leitor precisa ver esse passo.

Mire em cerca de 20 setas ou menos, porque um gráfico mais cheio é difícil de ler. Primeiro retorne rótulos mais grossos de `state()`. Dê um rótulo só a valores que agem igual, e deixe de fora um detalhe que o leitor não precisa, tipo uma chamada em voo cujo retorno não muda nada. Mantenha cada estado de um caminho de bug, e mantenha cada marca. Quando um merge muda um estado `breaks` para `mayBreak`, ele esconde onde uma regra quebra. Não funda dois estados que quebram regras diferentes, porque o leitor então não vê qual regra quebra onde. Se o gráfico ainda estiver cheio demais, retorne `null` de `step()` para os passos que o leitor não precisa, tipo um reset que todo estado tem, ou um passo cuja seta repete o que outras setas mostram. Obedeça estas regras quando esconder passos:

- Mantenha cada estado do gráfico alcançável por setas a partir de um estado inicial.
- Mantenha pelo menos uma seta saindo de cada estado do gráfico que tem um passo de saída no modelo.
- Esconda todos os passos de uma seta, ou nenhum. Quando você esconde só alguns, a condição dela fica mais estreita.
- No repasse, diga quais passos o gráfico esconde.

## Leia o relatório

- No gráfico, um estado que quebra uma regra tem a classe `breaks`. Quando só alguns estados do modelo por trás dele quebram a regra, ele tem a classe `mayBreak`. A Legenda nomeia as regras de cada marca. Quando duas regras quebram, ela nomeia as duas numa marca, então diga no repasse qual estado quebra qual regra. Num gráfico de regiões, um estado recebe uma marca quando toda combinação de estados de região que o inclui pode quebrar uma regra. Quando uma combinação pode quebrar uma regra e nenhum dos estados dela tem marca, cada estado dessa combinação recebe uma marca.
- O TLC segue além de um estado que quebra uma regra, então o gráfico mostra os estados depois de um bug.
- Como o TLC segue além dos bugs, um modelo sem limite nos estados dele pode rodar sem fim. Mantenha as constantes pequenas, ou adicione um CONSTRAINT.
- Com um VIEW ou uma SYMMETRY, o TLC conta como um só os estados que ele trata como iguais, e o gráfico desenha esse único estado. Uma seta pode levar a ele em vez do estado que o passo alcança, e o relatório diz isso.
- O TLC checa as regras em cada estado além de um CONSTRAINT ou de um ACTION_CONSTRAINT, mas não explora esse estado nem o checa para deadlock. Quando o caminho mais curto do TLC para uma regra termina num estado assim, o gráfico desenha esse estado, e o relatório dá esse caminho. O gráfico não desenha os outros estados além do limite. Um deadlock num estado assim não aparece. O TLC também checa as regras depois de cada passo que um ACTION_CONSTRAINT exclui, inclusive quando chega ao estado seguinte por outro caminho. Quando o caminho mais curto do TLC para uma regra termina num passo assim, o gráfico desenha esse passo.
- O comando checa as regras com o TLC na config como escrita, com um worker. Quando uma regra quebra, o comando remove essa regra e roda o TLC de novo, até nenhuma regra quebrar. Depois da primeira quebra, cada execução para em 100.000 estados distintos, para um modelo grande não rodar sem fim. Quando uma execução chega a esse limite ou para num erro, o relatório lista as regras que não checou em "Rules not checked", e o gráfico não as marca. Uma regra que não foi checada pode valer ou quebrar. Para checar, remova da config as regras que quebram, ou diminua o modelo. O limite adiciona um módulo que muda a ordem das variáveis do TLC, então para uma property o TLC pode achar um contraexemplo diferente de uma execução sem o limite.
- `tlc.out` é o que o TLC imprimiu na config como escrita. Quando o gráfico não mostra o caminho de bug de uma regra que quebra, `tlc-RULE.out` é o que o TLC imprimiu na execução que o achou. Quando o TLC repete o primeiro aviso dele, o TLC puro imprime duas vezes, mas `tlc.out` mostra uma vez, porque o TLC deixa a segunda cópia fora da saída que este comando lê. Isso só acontece quando o Java roda com `-XX:+UseParallelGC`, já que fora disso o primeiro aviso do TLC é o conselho de passar essa flag, que ele imprime uma vez.

## Mostre o gráfico

Mostre `chart.mmd` para o usuário como bloco de código `mermaid`. O gráfico tem um orçamento de 500 estados, contados pelo TLC na config como escrita. Acima disso, o gráfico desenha só os caminhos até as regras que quebram, e quando nenhuma regra quebra, o comando não escreve `chart.mmd`. Ele ainda checa cada regra, e o relatório dá a contagem de estados e cada caminho de bug. Para ver o grafo inteiro, diminua o modelo.

Renderize gráficos com Pele. Quando o usuário pede uma imagem, ou um gráfico é grande demais para ler como texto, siga `references/render.md`. Não use outro renderizador a menos que o usuário peça.

## Repasse

Entregue cada gráfico e o texto Mermaid dele para a pessoa que precisa decidir. Acrescente uma linha para cada bug. A regra, a frase do leitor para essa regra, e o caminho como o relatório o numera. Diga o que o modelo deixa de fora. Dê o comando que gera os arquivos de novo, com uma pasta `--out` nova.

Quando o modelo descreve código, reexecute cada caminho de bug contra o código real, como teste ou repro ao vivo, um passo para cada ação. Se um bug não ocorre, pense que o modelo está errado até você mostrar que não está.
