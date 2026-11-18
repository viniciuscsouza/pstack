---
name: create-formal-verification-skill
description: "Gere uma skill local do projeto que verifica formalmente o código onde um bug custa mais caro e os testes chegam menos, em qualquer linguagem, com a ferramenta de verificação que couber. Use para /create-formal-verification-skill, \"model-check this race\", \"prove this invariant\", ou quando concorrência, retentativas, máquinas de estado, parsers ou código unsafe precisarem de mais que testes."
disable-model-invocation: true
---

# Criar uma skill de verificação formal

Um teste checa os casos que alguém lembrou de escrever. Verificação formal checa todo caso num escopo declarado. Toda ordem de passos num modelo, toda entrada até um limite, ou toda entrada. Esta skill encontra a parte do repositório onde isso importa mais, verifica uma propriedade dela, e deixa uma skill local do projeto que permite ao próximo agente checar uma mudança contra essa propriedade. Escreva essa skill para um agente que a lê do zero, no meio de uma mudança, e nunca viu o seu trabalho. O usuário precisa conseguir confiar no resultado e agir sobre ele sem aprender a linguagem de verificação.

## Escolha o alvo a partir do código

Responda a partir do código, e pergunte ao usuário só o que você não consegue observar. Se o usuário nomeou uma área ou um bug, comece por ali.

Um bom alvo é uma promessa que o código faz, onde uma quebra prejudica usuários e os testes raramente alcançam o caso que quebra. Escolha o alvo mais forte desse tipo no repositório, não o mais fácil de verificar.

## Deixe a pergunta escolher a ferramenta

Escolha a ferramenta que responde à pergunta com o menor custo. Um model checker, um assistente de prova, um verificador com suporte a SMT, ou outra ferramenta. Se eventos podem acontecer numa ordem ruim costuma pedir um model checker sobre um modelo pequeno do design, e TLA+ é o padrão para isso em qualquer linguagem. Se o código em si se comporta para toda entrada, como em segurança de memória, overflow ou um invariante de estrutura de dados, costuma pedir um verificador que roda sobre o código. Se nenhum verificador roda nesta linguagem, diga isso, e teste o código real com entradas geradas, não um modelo manual da função. Uma propriedade que precisa valer em todo tamanho, onde um limite não basta, pode exigir uma prova. Prefira uma ferramenta que os desenvolvedores do repositório consigam instalar e rodar, e diga ao usuário por que você a escolheu.

## O que faz uma boa verificação

- **Fiel ao código.** Cada parte do modelo ou da prova aponta para código que um revisor consegue abrir. O que a verificação trata como um passo indivisível é um passo indivisível no código. Os nomes vêm deste repositório.
- **Capaz de falhar.** Uma checagem que você nunca viu falhar não prova nada. Mostre cada propriedade falhando numa variante quebrada do código que ela guarda, como uma checagem removida ou um passo reordenado.
- **Achados continuam achados.** Quando o código atual quebra a propriedade, relate com o contraexemplo nos termos do código. Não enfraqueça a propriedade nem o modelo até passar, e não mude código de produto a menos que o usuário peça.
- **Honesta sobre o escopo.** Diga o que está verificado, sob quais limites e suposições, e o que fica de fora. Um passe cobre o modelo ou o limite, não o sistema inteiro.
- **Legível sem a ferramenta.** Declare a propriedade e o dano em uma frase simples cada. Conte um contraexemplo como uma sequência curta de passos nas palavras do código.
- **Barata de rodar de novo.** Um comando, uma versão reprodutível da ferramenta, e uma execução curta o bastante para o próximo agente usá-la em toda mudança relevante.
- **Mantida em sincronia com o código.** Registre quais arquivos e funções cada checagem cobre, e o commit em que você confirmou pela última vez que a verificação ainda os descreve. Quando o código coberto muda depois desse commit, o próximo agente consegue ver que a verificação precisa de outra olhada.

## Gere a skill de verificação

Escreva uma skill local do projeto com frontmatter que nomeia a área e diz quando usá-la. Sem frontmatter, a skill nunca se registra. Somente pela skill, o próximo agente precisa conseguir dizer se uma mudança toca o código verificado, rodar a checagem, distinguir um bug real de um erro de modelagem, e atualizar a verificação quando a mudança é intencional. Ponha o registro de cobertura e o commit de sincronia na skill, e diga ao próximo agente para avançar o commit de sincronia só depois de confirmar que a verificação descreve o código no novo commit. Use os comandos, caminhos e substantivos reais deste repositório, e não deixe placeholders.

Se a checagem é um modelo TLA+, diga ao próximo agente para ler todo resultado com a skill [`/statechart`](../statechart/SKILL.md) do pstack. Ela mostra cada violação de invariante que o checker encontra como o seu caminho mais curto em palavras simples. Quando a checagem passa, `/statechart` desenha um gráfico de como o sistema se move entre estados. Refira-se a `/statechart` pelo nome. Os comandos, opções e caminhos de arquivo dela mudam entre versões e máquinas, então não copie nenhum deles para a skill gerada nem para os scripts dela.

## Prove antes de entregar

Siga a skill gerada uma vez, como o próximo agente faria, numa mudança descartável que quebra a propriedade no código real. Ela precisa pegar a quebra. Depois descarte a mudança descartável e confirme que a checagem voltou a onde começou. Passando, ou falhando só nos achados que você vai relatar. Uma skill de verificação que nunca rodou é um rascunho. Se a skill perde a quebra ou um passo não roda, conserte a skill ou a verificação, não a mudança descartável.

## Entregue

Diga ao usuário o que você verificou, qual ferramenta usou e por quê, os limites, o que a checagem encontrou em palavras simples, como rodá-la, e o que ela não cobre. Sugira um segundo alvo só se você encontrou um alvo próximo. Aponte o usuário para `/maintain-formal-verification-skill` para realinhar a verificação depois de mudanças que não a rechecaram.
