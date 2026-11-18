---
name: principle-make-operations-idempotent
description: "Aplique ao desenhar comandos, passos de ciclo de vida, ou laços de processamento que rodam em meio a quedas, reinícios e retentativas. Convirja para o mesmo estado final independentemente de execuções parciais anteriores."
disable-model-invocation: true
---

# Torne Operações Idempotentes

Desenhe toda operação que muda estado para convergir ao estado correto por quantas vezes rodar e onde quer que a execução anterior tenha parado.

Derive cada passo do estado que existe, não do que a execução anterior pretendia fazer. Leia o estado atual, compare com o objetivo, e aplique só a diferença.

Qualquer coisa que uma execução deixa para trás (um lock, um arquivo temporário, um registro pela metade, um job reivindicado) precisa ser reconhecível como obsoleta pela próxima execução, por evidência que vale onde quer que a execução anterior tenha parado. Escreva num caminho temporário e renomeie para o lugar.

O teste. O que acontece se isto rodar duas vezes? E se a execução anterior caiu em todo ponto possível? Se a resposta depende do que ficou para trás, adicione um passo de reconciliação.
