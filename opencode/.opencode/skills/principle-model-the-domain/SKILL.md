---
name: principle-model-the-domain
description: "Aplique ao escrever lógica com estado, ou quando o código ramifica muito ou repete uma suposição de forma entre arquivos."
---

# Modele o Domínio

Codifique o domínio numa estrutura de dados em vez de espalhar ele por condicionais. A estrutura certa torna estados inválidos irrepresentáveis e apaga branches.

Recorra a uma máquina de estados em vez de booleanos espalhados ou checagens de fase, a um modelo tipado em vez de parâmetros frouxos, a um mapa, registro ou união discriminada em vez de ramificação espalhada entre arquivos, a um reducer em vez de mutações ad hoc, e a um índice, fila ou coleção normalizada onde o padrão de acesso pede.

Não force. Mantenha código chato quando a forma é clara, local, e improvável de crescer, e desconfie de uma abstração que adiciona indireção sem remover branches ou estados inválidos.

O sinal de que você pulou isso. Uma funcionalidade que aumenta uma cadeia if/else com mais um branch, ou um segundo booleano que precisa ficar em sincronia com o primeiro.
