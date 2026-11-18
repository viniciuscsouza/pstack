---
name: principle-foundational-thinking
description: "Aplique antes de escrever lógica: escolher tipos e estruturas de dados centrais, sequenciar trabalho de andaime e de funcionalidade, perguntar o que atores concorrentes compartilham. Acerte as estruturas de dados para o código a jusante ficar óbvio."
---

# Pensamento Fundacional

Acerte a forma dos dados antes da lógica. Defina tipos centrais cedo, rastreie todo padrão de acesso, e escolha estruturas que casam com os caminhos dominantes. DRY na estrutura, não em cada linha.

Antes de compartilhar estado entre atores, pergunte o que acontece se outro ator modificar ele em paralelo. Se não for "nada", isole.

Andaime primeiro. O que ajuda fases posteriores (CI, testes, tipos compartilhados) vem antes, depois de remover código morto. Mantenha commits pequenos e de propósito único.
