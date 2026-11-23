---
name: how
description: "Use para \"how does X work\", walkthroughs de código antes de mudar algo, e dúvidas de lugar, dono e camada (\"onde isso deve morar\", \"qual pacote é dono disso\", \"esta é a camada certa\"). Explica arquitetura de subsistema, fluxo de runtime, modelos mentais de onboarding. Use why para motivação."
---

# Como

Explore a base de código para responder perguntas do tipo "como X funciona?".

Se o escopo estiver ambíguo, declare a sua interpretação e explore. O usuário pode redirecionar.

## 1. Explique

Crie um subagente Task com `subagent_type: generalPurpose` e `readonly: true` que explora e explica numa passada. Dê a ele a pergunta do usuário e este briefing:

> Você está escrevendo uma explicação arquitetural para um engenheiro sênior. Explore a base de código e escreva uma explicação que um engenheiro sênior sem familiaridade com esta área consiga ler e sair com um modelo mental sólido. Ele deve entender a arquitetura o suficiente para começar a trabalhar nela com confiança. Você tem acesso só de leitura à base de código.

## 2. Apresente

Apresente a saída do subagente. Edições leves para clareza ou contexto da conversa estão bem. Não reescreva de forma substancial.
