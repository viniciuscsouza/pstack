---
name: principle-laziness-protocol
description: "Aplique ao refatorar, avaliar o tamanho de um diff, ou na tentação de adicionar abstrações, camadas, ou repasse de sinal. Penda para a deleção e para a menor mudança que resolve o problema."
disable-model-invocation: true
---

# Protocolo da Preguiça

- **Consolide decisões.** Não repita a mesma escolha em vários lugares. Ponha ela atrás de uma única fonte de verdade e passe o resultado como uma flag simples.
- **Minimize o diff.** Faça a menor mudança que resolve o problema. Menos linhas vencem boilerplate "elegante".
