---
name: ketchup
description: "Atualize o usuário sobre o que mudou neste chat desde a última mensagem dele, em palavras simples e visuais pequenos. Liste todo item esperando ele, com o que ele precisa para decidir só pela resposta. Use para /ketchup."
---

# Ketchup

Atualize o usuário sobre o que mudou neste chat desde a última mensagem que ele escreveu antes de `/ketchup`, e ajude-o a entender cada mudança, não só onde ela está. Depois ponha na mesa toda decisão que espera ele, para que ele decida só com a sua resposta. Notificações e resultados de subagente não são mensagens dele. Assuma que o usuário lembra da última mensagem dele e de nada depois.

Use a voz de [`/bro`](../bro/SKILL.md), não a tarefa dele. Palavras simples, frases curtas, sem jargão, como uma pessoa atualizando outra. Diga o que mudou para o usuário, não como você fez. Diga "os testes passam", não "o CI está verde". Deixe de fora hashes, caminhos de arquivo, comandos, números de build e contagens de teste, a menos que o usuário precise de um deles para agir. Chame cada PR, branch ou agente por um nome que o usuário conheceria, não por um rótulo que você inventou. Se um fato precisa de contexto para fazer sentido, dê o contexto ou deixe o fato de fora.

## Junte o que mudou

Mantenha esta rodada só de leitura. Faça visuais apenas com o que você já tem. Não mude código, não suba um app ou um job, não poste nada. Junte o seu próprio trabalho desde aquela mensagem, relatórios de subagentes, e mensagens de qualquer outra pessoa neste chat. Cheque o estado atual de cada PR, execução de CI ou agente que este chat tocou. Cheque cada número que você vai mostrar contra os fatos de onde ele vem, e diga quando corrigir um.

Um item de ação é uma decisão ou passo que só o usuário pode tomar, como uma escolha de produto, uma aprovação, uma ação irreversível, ou um acesso que você não tem. Inclua todo item que ainda espera o usuário, mesmo um levantado antes da última mensagem dele. Trabalho que você ainda pode fazer sozinho não é item de ação. Nunca invente uma decisão para encher a resposta. Um passo que o usuário não pediu e o trabalho não precisa não é item de ação, a menos que seja irreversível e esteja esperando o usuário. Dê uma linha e nenhum visual.

Uma decisão pode cobrir muitos itens, como cada PR de um conjunto esperando merge. Para cada item, junte o link dele, o que ele muda para os usuários, a evidência por trás da sua escolha, o risco dele, e a sua escolha. Escreva "desconhecido" para um fato que a decisão precisa e você não sabe. Deixe de fora um link que você não tem.

## Escreva a resposta

Gaste palavras nas decisões, não na história. Mantenha o que aconteceu abaixo de 100 palavras, sem contar imagens, e mais curto quando menos aconteceu. Deixe de fora trabalho que não mudou nada para o usuário. Dê cerca de 30 palavras a cada item. A resposta cresce com o número de itens. Dois itens precisam de bem menos de 150 palavras no total, sem contar imagens, e trinta precisam de muito mais. Nunca corte itens nem os encolha para caber num limite de palavras. Nenhuma contagem no lugar de uma lista, e nenhum link no lugar de um item. Um link pode adicionar profundidade, mas a resposta sozinha precisa bastar para decidir.

Abra com uma frase curta que nomeie o que espera o usuário, ou diga que nada espera. Os itens de ação dão o detalhe. Se nada aconteceu e nada espera o usuário, diga isso e pare.

Depois mostre o que aconteceu, imagens primeiro. Embuta inline os screenshots e vídeos que o trabalho produziu, depois qualquer diagrama que explique uma mudança. Depois resuma em um bullet curto por coisa que o usuário pediu. O que saiu disso, e se está pronto, ainda rodando, falhou, ou não checado. Escreva cada linha do jeito que você diria em voz alta. "A busca estava lenta por falta de índice", não "Revisão da busca: feito" ou "Busca lenta: falta de índice". Nomeie qualquer bug achado e corrigido no caminho. Cada linha precisa continuar verdadeira lida sozinha. Nunca marque como pronto algo que ainda espera o usuário, e deixe de fora qualquer coisa que os itens de ação digam. Sob o resumo, dê no máximo três linhas curtas que mudam o que o usuário sabe ou precisa fazer. Depois nomeie em uma linha qualquer trabalho que sobrou e você pode fazer sozinho.

Depois liste os itens de ação. Peça a decisão que pertence ao usuário, como merge, aprovar, ou escolher, e dê a sua escolha. Um passo que você daria primeiro entra na sua escolha. Também qualquer coisa que precise acontecer antes, e qualquer coisa que não possa ser desfeita. Dê o número ou link de cada PR, issue ou agente sobre o qual é uma decisão, também numa decisão entre opções.

Ajuste a forma à decisão. Uma decisão sobre um item é um bullet. A decisão, a sua escolha, o que ela muda para os usuários, a evidência por trás da sua escolha, e o risco dela, com o item nomeado uma vez. Quando itens compartilham uma decisão, escreva a decisão e a sua escolha uma vez, depois dê a cada item o próprio bullet, mais arriscado primeiro. O link e o nome dele, o que ele muda para os usuários, a evidência por trás da sua escolha, e o risco dele. Nomeie a escolha de um item só onde ela difere da decisão. Dê o risco de cada item mesmo quando for baixo, como "Risco baixo: só as cores mudam". Diga o custo de esperar só quando for mais que um atraso, como um prazo ou dados perdidos.

Para uma decisão entre opções, dê cada opção que o trabalho pesou, com o que ela entrega ao usuário e o que ela custa, e o estado atual quando houver um. Esses custos são o risco da decisão, então não acrescente linha de risco separada. Quando os screenshots acima mostram as opções, escreva o ganho e o custo de cada opção sob o screenshot dela.

## Visuais

Um visual é uma imagem. Um screenshot, um vídeo, um diagrama Mermaid, um gráfico ou diagrama do `/visualize`, ou uma imagem gerada. Uma tabela Markdown não é um visual, mesmo com colunas de antes e depois.

Quando algo é mais fácil de pegar como imagem do que como frases, desenhe. Como um design ou sistema funciona, uma pilha de PRs ou cadeia de dependência com o estado de cada PR, um antes e depois de uma correção, ou opções lado a lado. Nunca desenhe uma lista de status, uma lista de itens de ação, ou caixas sem nada que as ligue. Nunca dê um fato duas vezes, no texto ou entre visuais.

Mantenha cada visual pequeno o bastante para ler de relance. No máximo seis caixas, com no máximo oito palavras em cada. Escreva rótulos como palavras simples que uma pessoa diria, não tags como "Testes: passam". Um fato que precisa de uma frase pertence ao texto, ou a lugar nenhum.

Embuta screenshots e vídeos inline quando a mudança é visível, não só como links. Se uma mudança visível não tem nenhum, marque a linha de resumo dela com "Sem screenshot ainda", e diga ali o que mudou na tela. Desenhe diagramas e gráficos a partir de fatos que você tem. Um diagrama Mermaid para como as coisas se conectam, e um gráfico do `/visualize` para números. Nunca invente um número, e nunca desenhe ou gere a imagem de uma tela que você não capturou. Linke só artefatos que existem.
