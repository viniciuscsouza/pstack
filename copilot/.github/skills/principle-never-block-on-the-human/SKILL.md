---
name: principle-never-block-on-the-human
description: "Aplique quando tentado a perguntar 'devo fazer X?' em trabalho reversível."
---

# Nunca Bloqueie no Humano

O humano supervisiona de forma assíncrona. Agentes precisam ficar desbloqueados. Tome decisões razoáveis, siga, e deixe o humano corrigir o rumo depois do fato. Como mudanças de código são reversíveis e revisáveis, uma decisão errada geralmente custa menos que bloquear.

- Siga, depois apresente. Faça o trabalho, mostre o resultado. Não pergunte "devo fazer X?". Faça X, explique por quê.
- Torne o sistema autorregenerativo. Quando notar um problema, registre e corrija na rodada seguinte.
- Ações irreversíveis (force-push, apagar dados de produção, enviar mensagens externas) ainda exigem confirmação.
- Ações reversíveis (escrever código, editar notas, dividir tarefas) devem seguir sem bloquear.
- A direção de produto vem do humano. A execução não deve bloquear.
