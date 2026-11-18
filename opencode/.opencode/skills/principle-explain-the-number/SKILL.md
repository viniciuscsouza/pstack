---
name: principle-explain-the-number
description: "Aplique antes de medir, e antes de confiar, relatar ou agir sobre um número que você mediu: um ganho de velocidade, uma regressão, um throughput, uma latência, ou um resultado de eval. Descubra o que o limita, e descarte que ele mediu algo diferente do trabalho que você pensa."
---

# Explique o Número

- **Escreva a decisão antes de rodar.** Nomeie o resultado que a mudaria, e o piso de ruído. Pare quando a dispersão entre execuções é maior que o efeito.
- **Pergunte "por que não o dobro?"** Nomeie o recurso ou caminho de código que limita o resultado, a partir de um profile ou de contadores tirados durante uma execução. Um palpite vindo da leitura do código não é limitador.
- **Liste o que mais o número pode estar medindo** (erros, trabalho pulado ou cacheado, código que nunca rodou, um lado sem ajuste, ruído, uma peça pequena demais para importar) e descarte cada um com evidência.
- **Guarde a contagem de execuções, a dispersão e o limitador junto do número.**

Para um número de performance, rode a skill [benchmark-checklist](../benchmark-checklist/SKILL.md). Para um resultado de eval, pergunte se toda execução fez a tarefa, se a diferença se sustenta entre tentativas e modelos, e se o cenário importa.
