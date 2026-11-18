---
name: principle-type-system-discipline
description: "Aplique ao desenhar tipos, revisar assinatura de função, ou escrever código em qualquer linguagem estaticamente tipada. Torne estados ilegais irrepresentáveis, marque primitivos semânticos, faça parse de dados externos nas fronteiras, esgote variantes, derive de schemas autoritativos."
---

# Disciplina de Sistema de Tipos

Faça o compilador rejeitar estados impossíveis, primitivos trocados e variantes não tratadas.

- **Marque primitivos semânticos.** Se dois argumentos têm o mesmo primitivo mas significam coisas diferentes, dê a cada um seu próprio tipo. Valide uma vez na criação.
- **Dado externo não tem tipo até o parse.** JSON, payloads de RPC, args de CLI, configuração, variáveis de ambiente e linhas de banco passam por uma função de parse na fronteira que devolve o modelo tipado.
- **Derive tipos de schemas autoritativos.** Quando um arquivo de schema é dono de uma forma, derive o tipo dele em vez de escrever um paralelo na mão.

Para sintaxe de TypeScript, veja a skill **typescript-best-practices**.
