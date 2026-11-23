---
name: blast-radius
description: "Descubra o que uma mudança pode quebrar em outro lugar antes da entrega, além do diff, e prove o único fato que a torna segura rodando código real em vez de escrever um relatório. Use para 'blast radius de X', 'o que isso pode quebrar', ou para revisar um diff pequeno em que você não confia."
---

# Raio de impacto

Descubra o que uma mudança quebra em outro lugar, antes da entrega. Use para "raio de impacto de X", "o que isso pode quebrar", ou para revisar um diff pequeno em que você ainda não confia.

Companheira de `how` e `why`. `how` diz o que o código faz. `why` diz por que ele tem essa forma. Raio de impacto diz o que ele quebra em outro lugar.

Listar os chamadores não é o trabalho. O agente faz grep disso num segundo. O trabalho é a quebra que o grep não mostra.

## Não confie no seu próprio relatório

Um relatório de raio de impacto que soa certo não vale nada. Ele parece convincente sendo verdadeiro ou não. Então não entregue o relatório. Ache os um ou dois fatos de que tudo depende e prove eles rodando código.

### Quão certo você está

Para cada fato de que a segurança da mudança depende, leve ele o mais fundo nesta lista que for barato, e diga onde parou.

1. Você afirmou. Sem valor sozinho.
2. Você apontou a linha. Um `file:line` real, ou o código-fonte da própria biblioteca.
3. Você mostrou que o caso ruim não acontece. Você percorreu a falha passo a passo e ela não chega.
4. Você rodou. Um script ou teste que chama o código real e falha alto se você estiver errado.
5. Você reproduziu no app rodando.

O passo 4 costuma ser um script pequeno que importa a mesma biblioteca que o app entrega e chama a função exata que te preocupa.

## Passos

1. Leia a mudança. O diff, os símbolos que ela adiciona, muda e deleta, e o que ela agora faz diferente, incluindo a parte que o diff não explicita. Puxe o PR e os commits dele.
2. Ache o único fato que a torna segura. A maioria das mudanças que parecem arriscadas é segura por causa de um fato só, tipo "esta chamada só descarta entradas de cache já mortas e não faz mais nada". Ache esse fato. Se ele vale, a maioria dos casos arriscados cai de uma vez. Gaste seu tempo aqui, não numa lista longa de talvez.
3. Olhe onde o grep para. Leia o código-fonte da biblioteca que você chama, e confira a versão fixada e qualquer patch local. Descubra quando as coisas rodam. Microtasks, unmount e teardown, Solid versus React. Siga o que uma busca por símbolo perde. O JSON que uma API retorna, uma coluna de DB, um formato de rede, outra linguagem lendo os mesmos bytes, uma feature flag, código três saltos abaixo.
4. Seja honesto sobre cada risco. Dê a ele uma chance real de acontecer e um custo real se acontecer. Mantenha os riscos que você confirmou. Liste à parte os que você checou e descartou. Cite um `file:line` real, uma busca que não acha nada ainda é uma resposta, e nunca invente um chamador ou uma API.
5. Prove o único fato. Escreva um script ou teste que roda o código real, rode, e cole o que aconteceu.
6. Para uma mudança grande ou ampla, rode como `arena`. Pergunte a mais de um modelo a mesma coisa e junte as respostas. Modelos diferentes pegam bugs reais diferentes.

## O que entregar

- **O que faz.** O que mudou, incluindo a parte que não é óbvia.
- **O único fato que a torna segura.** Enuncie, diga até qual passo você chegou, e mostre a prova. Se não conseguiu provar, escreva não provado.
- **Riscos.** Cada um nomeia como quebra, o `file:line`, quão provável e quão ruim, e como checar. Cole a prova dos que importam.
- **Descartados.** O que você checou e por que está tudo bem.
- **Antes do merge.** O teste ou repro mais barato que pega o bug real, incluindo o script que você escreveu.

Escreva via `unslop`, cite código real, e remova qualquer coisa privada antes de ele ir para qualquer lugar público.

**Resposta:** o relatório acima, com o único fato de segurança provado ou marcado como não provado.
