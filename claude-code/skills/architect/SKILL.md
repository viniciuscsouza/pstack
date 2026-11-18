---
name: architect
description: "Esboce tipos, assinaturas e estrutura de módulo antes do código, depois fique no circuito enquanto a implementação preenche. Use para /architect, \"architect this\", \"design this\", ou trabalho não trivial onde pular direto para o código travaria a forma errada."
disable-model-invocation: true
---

# Architect

Antes de escrever código, rastreie todo sistema que a mudança toca, depois esboce duas formas diferentes para ele (tipos, assinaturas, fronteiras de módulo) e escolha aquela onde cada pedaço de estado tem um dono, cada tarefa tem um jeito de fazer, e os chamadores precisam saber o mínimo. Redesenhe em vez de remendar se as gambiarras se repetirem.

Para um design grande ou contestado, rode a skill **arena** em vez de esboçar sozinho.
