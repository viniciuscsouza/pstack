---
name: principle-prove-it-works
description: "Aplique depois de concluir uma tarefa, antes de declarar pronto. Verifique contra o artefato real (rode a funcionalidade, leia o valor de fato, inspecione o diff), não contra um proxy, autorrelato ou 'compila'."
disable-model-invocation: true
---

# Prove que Funciona

Verifique toda saída checando a coisa real diretamente. Rode a funcionalidade, leia o valor de fato, inspecione o diff. Proxies (mtime de arquivo, autorrelatos, saída em cache, "compila") não contam. Quando uma checagem falha, suspeite do método de observação antes do sistema.

Scripte a checagem quando puder. Um script determinístico que refaz a comparação vence uma olhada única. Mantenha a saída dele onde o humano pode ver, e faça commit só quando a trilha precisa ser auditável depois.
