---
name: ketchup-video
description: "Alcance o usuário como o /ketchup, com um vídeo curto legendado de slides sobre o que aconteceu e as chamadas pendentes como texto embaixo. Use para /ketchup-video."
---

# Vídeo ketchup

Siga o [`/ketchup`](../ketchup/SKILL.md), mas mostre o que aconteceu num vídeo curto legendado em vez do resumo e dos visuais dele. A janela dele começa na última mensagem que o usuário escreveu antes de `/ketchup-video`, não antes de nenhum `/ketchup` anterior. Escrever e renderizar o vídeo, fora do repositório, é a única exceção à regra read-only dele. A resposta é a frase de abertura do ketchup, o vídeo embutido, uma linha nomeando qualquer trabalho que sobra e que você mesmo pode fazer, e os itens de ação. Quando o vídeo mostra as opções de uma chamada, chame cada opção pelo nome que o vídeo dá a ela. Se a renderização falhar, diga isso em uma linha e dê só as chamadas.

## Slides

Use os screenshots, gravações, diffs e dados que o trabalho produziu, não a prosa do chat sobre eles. Antes de desenhar, escreva a frase única com que o usuário deve ficar e uma linha por slide. Descarte cada slide que não muda o que o usuário sabe. Use no máximo 10 slides, e menos é melhor. Nenhuma das duas notas entra na resposta.

Cada slide faz um ponto. Construa os slides sobre uma imagem e mude ela de slide para slide, em vez de cortar para outra sem relação. Uma cor ou forma significa a mesma coisa em todo slide. Mostre o trabalho em imagens, como um sistema funciona, uma pilha de PRs com o estado de cada PR, um antes e depois de uma correção, ou opções lado a lado. Aponte o que mudou com setas, destaques e movimento. Um slide nunca é lista de marcadores, lista de status, tabela, ou caixas sem nada ligando elas.

Mantenha o texto no mínimo. Ponha cada rótulo na coisa que ele nomeia, em palavras simples que uma pessoa diria, não tags como "Tests: pass". Nunca legende o que uma imagem mostra, e nunca dê um fato duas vezes, entre slides ou no texto embaixo deles.

Mostre o que saiu de cada coisa que o usuário pediu, e se está feito, ainda rodando, falhou, ou não foi checado. Nunca mostre como feito algo que ainda espera o usuário. Mostre qualquer bug encontrado e corrigido no caminho. Se uma mudança visível não tem screenshot, diga isso no slide dela e nomeie o que mudou na tela. Desenhe só a partir de fatos que você tem. Nunca invente um número, e nunca desenhe uma tela que você não capturou.

## Legendas

Dê a cada slide uma linha curta de legenda, de cerca de dez palavras, que diz o ponto dele do jeito que você contaria ao usuário, só a partir do que aquele slide mostra. Diga o que a imagem significa em vez de ler os rótulos dela. Ponha a legenda numa faixa abaixo do slide, da mesma altura em todo slide e vazia exceto pela legenda, para ela nunca cobrir o slide. Defina o tamanho pequeno, mas legível quando o vídeo roda no tamanho do chat.

## Renderização

Renderize um vídeo silencioso em paisagem, como um mp4, com movimento entre os slides. Todo frame é o slide em si, de ponta a ponta, acima da faixa de legenda dele. Faça os frames a partir dos slides, nunca gravando uma tela, e queime cada legenda nos frames do slide dela para ela ficar na tela durante toda a permanência do slide. A permanência é o tempo em que um slide está inteiro na tela, sem contar o movimento de entrada ou saída. Segure cada slide o suficiente para ler as palavras que ele acrescenta, no mínimo um segundo para cada três palavras. Um slide acrescenta a legenda dele e cada palavra que não estava no slide anterior, incluindo um rótulo que ele muda. O primeiro slide acrescenta todas as palavras dele. Palavras dentro de um screenshot não contam.

Antes de embutir o vídeo, confira o arquivo renderizado. Olhe um frame de cada slide no tamanho do chat, e confira se a legenda dele é legível e não cobre nada. Meça cada permanência casando os frames do arquivo com o slide, não com um detector de frame parado, que conta as pontas fracas de um fade como paradas. Conte as palavras que cada slide acrescenta contra a permanência medida. Corrija qualquer slide que você não consegue ler nesse tempo, renderize de novo, e confira de novo.
