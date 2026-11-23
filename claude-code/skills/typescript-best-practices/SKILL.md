---
name: typescript-best-practices
description: Boas práticas de TypeScript. Use ao ler ou editar qualquer arquivo .ts ou .tsx.
paths: ["**/*.ts", "**/*.tsx"]
disable-model-invocation: true
---

# Boas práticas de TypeScript

Aplique a skill de princípio **type-system-discipline** primeiro.

| Regra | Resumo |
|------|---------|
| Uniões discriminadas | Modele variantes com um discriminante literal `kind` para que estados impossíveis não possam ser representados. Nada de sacos de campos opcionais. |
| Tipos marcados | Marque primitivos com `& { readonly __brand: "X" }` para que não possam ser confundidos. Valide uma vez na fronteira. Uma marca vem de um type guard verificador (`input is AgentId`), nunca de `as`. |
| Modelagem construtiva | Construa a forma para que o valor ilegal não possa ser construído. `[T, ...T[]]` para não vazio, `[T, T][]` para comprimento par, `start` mais `duration` para um intervalo. Não é guarda de runtime, nem desejo por tipos de refinamento. |
| Tipo total mais simples | Mantenha `T[]` enquanto toda operação sobre ele continuar total. Reforce para `NonEmpty<T>` só onde o tipo frouxo força `!`, um cast, ou um throw de "nunca deveria acontecer". |
| `unknown` em vez de `any` | Dado externo é `unknown`. |
| Schemas antes de guardas | Antes de escrever à mão um type guard propriedade por propriedade, use a biblioteca de schema de runtime do repositório e infera o tipo do schema, como `z.infer`. |
| Nada de casts `as` | Todo `as` é um crash de runtime esperando. Faça cast só depois da validação. |
| Hierarquia de narrowing | Switch de discriminante > operador `in` > `typeof`/`instanceof` > type guard do usuário > `as`. |
| Type guards | Precisa verificar a afirmação. Uma guarda mentirosa é pior que `as` porque o bug se esconde atrás de um nome que diz que é seguro. Nomeie elas `isX` ou `hasX`. |
| Exaustividade | Use `const _exhaustive: never = x;` inline nos braços default para que o compilador erre quando uma variante nova for adicionada. |
| `satisfies` em vez de `as` | Valida o valor sem alargar tipos literais. |
| Validação de fronteira | Faça parse onde o dado entra, para um tipo de domínio nomeado. `Record<string, unknown>` (como for escrito) para nesse parse. Confie nos tipos dentro. Veja a skill de princípio **boundary-discipline**. |
| Tipos derivados de schema | Recorra a `Pick`/`Omit`/`Parameters`/`ReturnType`/`Awaited`/`typeof` antes de declarar uma interface nova. |
| Argumentos em objeto | Passe objetos, não posicionais, para que a ordem dos argumentos se documente sozinha. Pule em caminhos quentes (render por frame, tokenizers, parsers). |
| Testes reais | Não faça mock do que você consegue rodar. Prefira as primitivas de teste reais do framework com checagens de vazamento e descarte, e verifique UI num build rodando. Faça mock só do que você não consegue rodar localmente. |
| Telemetria estruturada | Prefira diagnóstico de logger estruturado com contexto suficiente para depurar a partir de um id. Nada de `console.log` em código entregue. |

Exemplos em `references/patterns.md`.
