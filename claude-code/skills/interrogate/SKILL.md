---
name: interrogate
description: "Use para \"interrogate\", \"adversarial review\", \"multi-model review\", \"challenge this\", \"stress test this code\", \"find blind spots\", ou \"tear this apart\". Adversários tentam quebrar uma mudança ou um design, e só achados provados por uma execução sobrevivem."
disable-model-invocation: true
---

# Interrogue

Adversários tentam quebrar uma mudança ou um design. Um achado só sobrevive quando uma execução o prova. Relate os achados. Não aplique correções.

## Enquadre

O escopo é o que o usuário nomeia. Caso contrário é o diff do branch desde a merge base com o branch base, até a árvore de trabalho. Declare a intenção com a skill **in-your-own-words**, a partir da mensagem do usuário, dos commits e da descrição da PR. Pergunte ao usuário quando a intenção não está clara.

## Abra em leque

Rode os passos 3 e 4 da Fase A e a Fase B da skill **arena**, com quatro mudanças. Leia a linha `interrogate reviewers` no lugar de `arena runners`. Copie as mudanças não commitadas do escopo para cada worktree. Crie cada adversário como `generalPurpose`. No lugar da tarefa e do racional da arena, dê a cada adversário o escopo, a intenção, e este briefing:

> Quebre esta mudança. Prove cada achado com uma execução contra o código real, como um teste que falha, um script de repro, ou saída de comando que mostra o comportamento errado. Para um design ou plano, rode um protótipo ou sonde a árvore atual. Mantenha os repros no seu próprio worktree. Relate cada achado com um título, o comando exato, a saída que ele imprimiu, e a saída que a intenção exige. Deixe de fora qualquer coisa que você não rodou. Isso cobre estilo, nomes, estrutura, e riscos que precisam de uma mudança futura para importar. Nenhum achado é um resultado válido.

## Verifique

Verifique cada achado conforme a Fase F da arena, rodando o comando dele de novo. Mantenha um achado quando a saída mostra a falha e a falha quebra a intenção. Descarte o resto. Una achados que compartilham uma causa raiz.

Pule as Fases C, D e E da arena. Adversários acham falhas diferentes, então o veredito guarda todo achado verificado, não a lista de um adversário.

## Saída

- **Intenção**: a reformulação.
- **Achados**: para cada achado verificado, o comando, a saída dele, como ele quebra a intenção, e os adversários que o acharam. Diga "nenhum achado verificado" quando nenhum sobrevive.
- **Descartados**: para cada adversário, a contagem de achados descartados e por quê.
