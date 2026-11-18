---
name: principle-subtract-before-you-add
description: "Aplique ao sequenciar uma adição, refatoração ou reescrita. Remova código morto, validadores redundantes e referências stub primeiro, depois construa sobre a base mais simples."
---

# Subtraia Antes de Adicionar

Ao evoluir um sistema, remova complexidade primeiro, depois construa. Padrão é subtração.

Faça da simplificação um investimento contínuo. Deixe o design um pouco mais simples e mais capaz atrás da mesma superfície ou de uma menor do que a que você encontrou.

- Sequencie a remoção antes da construção
- Corte antes de polir (chegue ao mínimo antes de investir em qualidade)
- Projete para o uso observado, não para casos de borda especulativos
- Nenhum validador, parser ou guarda especulativo além do que a spec exige
- Simplifique prompts (remova instruções redundantes, templates excessivos)
- Quando uma referência não tem conteúdo novo, apague em vez de deixar um stub
