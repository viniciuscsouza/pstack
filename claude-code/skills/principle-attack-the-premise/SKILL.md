---
name: principle-attack-the-premise
description: "Aplique quando duas ou mais correções que compartilham uma premissa falharam no mesmo portão. Escreva a premissa e teste ela direto antes de escrever outra correção que a assume."
disable-model-invocation: true
---

# Ataque a Premissa

Quando duas ou mais correções que compartilham uma premissa falharam no mesmo portão, suspeite da premissa, não das correções.

- Escreva a premissa. A única frase que toda correção que falhou assumiu.
- Teste ela antes da próxima correção. Encontre uma observação que sairia diferente se a premissa fosse falsa, e faça essa observação. Olhe como as falhas se distribuem, não só o tamanho delas.
- Se a evidência contradiz a premissa, procure o que as falhas compartilham que ela não previu. Se uma parte sempre cai no lado perdedor, mude o que põe ela ali em vez de adicionar um mecanismo que limpa depois.

Não comece a próxima correção antes da premissa estar escrita e testada.
