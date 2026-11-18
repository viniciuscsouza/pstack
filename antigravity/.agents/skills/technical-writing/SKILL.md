---
name: technical-writing
description: "Padrão de escrita técnica em camadas. Estrutura Diátaxis, frases no estilo de desenvolvedor do Google, regras de instrução STE, sintaxe de Global English. Use para /technical-writing ou ao escrever ou revisar docs, RFCs, readmes, descrições de PR, ou mensagens de commit."
---

# Escrita técnica

Escreva para um engenheiro cansado entender na primeira leitura.

- Corte toda palavra que não faz trabalho.
- Use a palavra curta e cotidiana. "use", não "utilize".
- Quando uma regra piora uma frase, conserte a frase de outro jeito ou deixe como está.
- Escreva o nome real do símbolo, arquivo, flag, ou comando, não um sinônimo nem uma descrição dele.
- Não invente jargão. Use as palavras que um desenvolvedor diria em voz alta. Diga o que um padrão nomeado significa na primeira vez.

## Varie o ritmo

- Misture comprimentos de frase. Divida a frase que carrega dois pensamentos. Mantenha a frase longa que carrega um.
- Tenha uma visão na explicação. Mantenha a referência seca.
- Seja específico. Não "mudanças de schema podem causar problemas", mas "renomear uma coluna quebra o build".

## Escolha o modo primeiro (Diátaxis)

Um documento, um modo. Não misture modos. Divida e linke.

- **Tutorial. Aprender fazendo.** Diga o que o aprendiz vai construir. Todo passo produz um resultado visível, e você diz o que ele deve ver. A explicação ganha uma oração e um link.
- **How-to. Passos para um objetivo.** Resolva o problema do leitor. Assuma competência. Só ação. Permita bifurcações. Nomeie o guia pela tarefa.
- **Reference. Fatos para consulta.** Só descreva. Seja seco, completo e seguro. Espelhe a estrutura da coisa descrita.
- **Explanation. Entendimento e porquê.** Um tópico delimitado. Dê contexto. Decisões de design, histórico, restrições, alternativas. Opinião é permitida aqui e em nenhum outro lugar.

## Escreva frases para o leitor (estilo de desenvolvedor do Google)

- Fale com o leitor como "você", no presente.
- Diga quem faz o quê. A passiva só serve quando o ator é desconhecido ou irrelevante.
- Escreva instruções como comandos. Ponha a condição antes da instrução e o caso comum primeiro.
- Sem buzzwords, sem linguagem figurada, sem "por favor", e nunca "simplesmente", "fácil" ou "rapidamente" num procedimento.
- Não anuncie antes, e não comece frases consecutivas com a mesma expressão.
- Linke com palavras que dizem para onde o link vai.
- Títulos carregam o ponto, em sentence case. Um h1 por página, sem níveis pulados.
- Listas numeradas para sequências, bullets para o resto. Introduza uma lista com uma frase completa. Mantenha os itens paralelos.
- Código vai em fonte de código. Elementos de UI vão em negrito. Use vírgula serial. Corte "etc.".

## Faça cada afirmação carregar uma coisa por vez (regras STE)

- Uma instrução por frase. Um pensamento por frase em todo o resto.
- Divida instruções com mais de cerca de 20 palavras e outras frases com mais de cerca de 25.
- Ponha o aviso ou a condição antes do passo que ele protege.
- Mantenha "o", "a" e "um".
- Dê um significado a cada palavra, e escolha uma palavra por ação.
- Escreva procedimentos como comandos diretos, nunca como narração e nunca na passiva.
- Evite palavras terminadas em "-ndo" onde puder.

## Não deixe nenhuma frase aberta a duas leituras (Global English)

- Mantenha palavras como "só" e "não" perto da palavra que elas mudam.
- Quebre cadeias longas de substantivos.
- Faça todo "ele", "eles" e "isto" apontar para uma coisa óbvia. Nunca use "isto" ou "que" para apontar para uma oração inteira.
- Não corte verbos, nem as palavras pequenas que mostram estrutura, como "que".
- Repita o artigo numa série quando isso evita uma leitura errada. Diga quais partes "e" ou "ou" une.
- Use pontos, não ponto e vírgula nem travessão longo. Sem barras, e sem "(s)". Faça o texto entre parênteses uma unidade gramatical completa.
- Chame cada coisa por um nome, em todo lugar.
- Deixe de lado idiomas, coloquialismos, abreviações latinas, e metáforas.

## Voz e particularidades do repositório

- Aplique a skill **unslop** a todo doc que esta skill toca. Essa skill é dona do catálogo de padrões de slop. Vocabulário de IA, enchimento, hesitação, marcas de formatação.
- Descrições de PR e mensagens de commit são escrita também. Toda camada menos Diátaxis se aplica a elas. O corpo de uma PR é um briefing que um revisor lê em menos de um minuto. Não cole logs de swarm, listas de SHA, ou tabelas de métricas. Linke.
- Strings de UI de produto não são documentação. Use as diretrizes de copy do seu produto para elas.
- Indente trechos de código com tabs. Escreva caminhos reais. Faça toda afirmação de contagem ou de árvore ser verdadeira no commit que a entrega, e inclua o comando que a regenera.
