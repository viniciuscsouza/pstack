---
name: principle-minimize-reader-load
description: "Aplique ao revisar ou moldar código difícil de rastrear. Conte camadas entre a pergunta e a resposta, e estado escondido na cabeça do leitor; colapse wrappers de um único chamador e encolha o escopo mutável."
disable-model-invocation: true
---

# Minimize a Carga do Leitor

Carga do leitor é o trabalho que o leitor faz para entender código. Ela tem dois eixos independentes. Camadas a rastrear entre a pergunta e a resposta, e estado escondido ou mutável a segurar na cabeça. Proteja os dois.

- Colapse camadas que só repassam, wrappers com um chamador, adaptadores sem segunda implementação, métodos de passagem. Inline eles.
- Encolha o escopo do estado, retornos em vez de mutações, locais em vez de campos, campos em vez de estado de módulo, estado de módulo em vez de globais. Derive em vez de sincronizar.
- Nomeie um invariante uma vez na fronteira, não em todo consumidor.

O teste é simples. Um leitor novo consegue responder "de onde vem X?" e "o que pode mudar X?" em menos de 30 segundos? Se não, corte camadas ou corte estado.