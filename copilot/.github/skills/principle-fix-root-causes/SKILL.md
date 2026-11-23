---
name: principle-fix-root-causes
description: "Aplique ao depurar. Rastreie cada sintoma até o primeiro ponto onde o sistema errou e corrija ali. Uma guarda só é correção quando o estado que ela trata é legítimo."
---

# Corrija a Causa Raiz

Corrija a falha, não o lugar onde ela apareceu. A causa raiz é o primeiro ponto onde o sistema fez algo que não devia, ou deixou de tratar um estado que devia tratar.

- Reproduza primeiro. Pergunte "por quê" até a resposta ser uma decisão errada no código. Quando travar, instrumente e leia o estado real.
- Pergunte se o estado que disparou o sintoma é legítimo. Se for, trate no ponto onde o código quebrou. Se ele nunca devia existir, uma checagem ali esconde o bug, então ache o que o produziu. Guardas, retentativas, erros engolidos, valores limitados e estado descartado são as formas usuais de esconder um bug.
- Corrija o padrão, não a instância. Ache todo lugar com a mesma falha.
- Se o fato decisivo exigiu uma escavação funda, proponha o sinal barato e sempre ligado que o teria mostrado à primeira vista.
