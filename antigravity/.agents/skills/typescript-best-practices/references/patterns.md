# Padrões de TypeScript

Exemplos de código para cada regra em `SKILL.md`. Os princípios por baixo são agnósticos de linguagem. Veja as skills de princípio **type-system-discipline** e **boundary-discipline**.

## Tipos marcados (branded types)

Marque primitivos para que não possam ser confundidos. Valide uma vez na fronteira. O código a jusante confia no tipo.

```ts
type AgentId = string & { readonly __brand: "AgentId" };

function isAgentId(input: string): input is AgentId {
  return isUUID(input);
}

function parseAgentId(input: string): AgentId {
  if (!isAgentId(input)) throw new Error(`Invalid agent id: ${input}`);
  return input;
}

function focusAgent(id: AgentId): void {
  /* a entrada é confiável */
}
```

Siga a forma `readonly __brand: 'X'`. Não invente uma convenção nova.

## Uniões discriminadas

Modele variantes com um discriminante literal. Toda variante compartilha o nome do campo e o valor de cada variante é único, então combinações impossíveis não podem ser representadas.

```ts
// Não. Booleano + opcionais deixa estados contraditórios existirem.
type DiffState = { loading: boolean; diff?: GitDiff; error?: string };

// Faça. Só estados válidos existem.
type DiffState =
  | { kind: "loading" }
  | { kind: "ready"; diff: GitDiff }
  | { kind: "error"; error: string };
```

Escolha um nome de discriminante (`kind`, `type`, `tag`) e mantenha ele.

## Modelagem construtiva

Construa o tipo a partir de partes todas legais em vez de restringir um tipo frouxo com checagens de runtime.

Não vazio, via tupla variádica:

```ts
type NonEmpty<T> = [T, ...T[]];

// Não: T[] mais uma checagem de tamanho que todo chamador precisa repetir
function pickWinner(entries: string[]): string {
  if (entries.length === 0) throw new Error("no entries");
  return entries[Math.floor(Math.random() * entries.length)];
}

// Faça: um valor vazio do tipo não pode existir
function pickWinner(entries: NonEmpty<string>): string {
  return entries[Math.floor(Math.random() * entries.length)];
}
```

Onde um `T[]` puro chega, estreite uma vez com uma guarda. O fato então viaja no tipo:

```ts
const isNonEmpty = <T>(arr: T[]): arr is NonEmpty<T> => arr.length > 0;
```

Tamanho par, como pares:

```ts
type Pairs<T> = [T, T][];
```

Um intervalo de tempo, como início mais duração:

```ts
// Não: um comentário sustenta o invariante
type TimeRange = { start: Date; end: Date }; // start <= end

// Faça: um intervalo negativo não pode ser escrito; derive o fim quando precisar
type TimeRange = { start: Date; durationMs: number };
```

Mantenha `durationMs` um número puro. Marque ele (conforme Tipos marcados) só se um número cru puder ser passado onde uma duração é esperada, não por reflexo. Escolha a representação que torna o estado ruim não construível, depois exponha a leitura que você precisa por cima (`pairs.flat()`, um helper `rangeEnd()`).

## O tipo total mais simples

Não reforce tudo. Mantenha `T[]` quando toda operação sobre ele é total:

```ts
const sum = (xs: number[]) => xs.reduce((a, b) => a + b, 0); // [] é 0, tudo bem
```

Reforce quando o tipo frouxo força uma mentira num ponto de uso. Os sinais são `!`, `arr[0] as T` e um throw de "nunca deveria acontecer":

```ts
// Não: parcialidade contrabandeada por fora do compilador
function newestSession(sessions: Session[]): Session {
  return sessions.at(0)!;
}

// Faça: reforce a entrada; a asserção desaparece
function newestSession(sessions: NonEmpty<Session>): Session {
  return sessions[0];
}
```

Enfraquecer o resultado para `Session | undefined` é a outra assinatura total.

## `unknown` em vez de `any`

Dado externo é sempre `unknown`. Estreite antes de usar.

```ts
// Não
function handle(input: any) {
  return input.foo.bar;
}

// Faça
function handle(input: unknown) {
  if (typeof input === "object" && input !== null && "foo" in input) {
    // estreitado; o compilador verifica o acesso
  }
}
```

Fontes externas incluem payloads de RPC, `JSON.parse`, `postMessage`, IPC, conteúdo de arquivo, variáveis de ambiente, resultados de banco.

## Schemas antes de guardas feitas à mão

Antes de escrever uma guarda de tipo propriedade por propriedade para dado externo, procure a biblioteca de schema de runtime do repositório e os schemas existentes. Deixe um schema ser dono da validação e derive dele o tipo TypeScript. Não mantenha um schema, uma interface duplicada e uma guarda que podem divergir.

```ts
import { z } from "zod";

const UserSchema = z.object({
  id: z.string().uuid(),
  role: z.enum(["admin", "member"]),
});

type User = z.infer<typeof UserSchema>;

function parseUser(input: unknown): User {
  return UserSchema.parse(input);
}
```

Use `safeParse` quando a falha é um ramo esperado. Use o helper de inferência equivalente quando o repositório usa outra biblioteca de schema. Não adicione uma dependência de schema nova por causa de uma guarda. Esta regra prefere o sistema de schema em que o código já confia.

## Sem casts `as`

Todo `as` é uma queda de runtime em potencial. Faça cast só depois que o sistema de tipos verificou a afirmação.

```ts
import { z } from "zod";

// Não
const user = data as User;

// Não
function isUser(data: unknown): data is User {
  return typeof data === "object" && data !== null && "id" in data;
}

// Faça
const userSchema = z.object({ id: z.string(), name: z.string() });
type User = z.infer<typeof userSchema>;

function parseUser(data: unknown): User {
  return userSchema.parse(data);
}
```

Quando o tipo vem primeiro, anote o validador com o tipo que ele prova. O compilador então rejeita um validador que prova menos que o tipo. Remova `name` do objeto abaixo e a atribuição deixa de compilar.

```ts
type User = { id: string; name: string };

const userSchema: z.ZodType<User> = z.object({ id: z.string(), name: z.string() });
```

Ao refatorar um `as` para fora de código existente, identifique por que o TypeScript não consegue inferir:

- Discriminante ausente. Adicione um, troque para uma união discriminada.
- Tipo de origem largo demais (ex. `Record<string, unknown>`). Estreite.
- Fronteira sem tipo. Faça parse com o schema dono da forma. Adicione um schema só onde não existe nenhum.
- Realmente inexprimível. Use um tipo marcado ou `satisfies`.

## Hierarquia de estreitamento

Do melhor para o último recurso:

1. **Switch / if de união discriminada.** O compilador estreita sozinho.
2. **Operador `in`.** `"key" in obj` estreita para as variantes que contêm essa chave.
3. **`typeof` / `instanceof`.** Para primitivos e instâncias de classe.
4. **Guarda de tipo definida pelo usuário.** Quando o acima não basta.
5. **Cast `as`.** Só depois da validação.

```ts
function area(s: Shape): number {
  if ("radius" in s) return Math.PI * s.radius ** 2; // estreitado para círculo
  return s.width * s.height; // estreitado para retângulo
}
```

## Guardas de tipo

Uma guarda precisa verificar de fato a afirmação. Uma guarda mentirosa é pior que `as`.

```ts
function isCircle(s: Shape): s is Shape & { kind: "circle" } {
  return s.kind === "circle";
}
```

Prefira o estreitamento por discriminante quando possível.

## Exaustividade

Nos ramos default, atribua o discriminante a uma local tipada como `never`.

```ts
// Switch que retorna valor
function area(s: Shape): number {
  switch (s.kind) {
    case "circle":
      return Math.PI * s.radius ** 2;
    case "rect":
      return s.width * s.height;
    default: {
      const _exhaustive: never = s;
      return _exhaustive;
    }
  }
}

// Switch void
function handle(s: Shape): void {
  switch (s.kind) {
    case "circle":
      drawCircle(s);
      break;
    case "rect":
      drawRect(s);
      break;
    default: {
      const _exhaustive: never = s;
      void _exhaustive;
    }
  }
}
```

Estilo de retorno em switches que retornam valor, estilo void em switches de instrução.

## `satisfies` em vez de `as`

`satisfies` valida sem alargar tipos literais.

```ts
// Não. Alarga, perde os tipos literais.
const config = { theme: "dark", cols: 3 } as Config;

// Faça. Valida E preserva os tipos literais.
const config = { theme: "dark", cols: 3 } satisfies Config;
// config.theme é "dark" (literal), não string
```

## Validação de fronteira

Valide uma vez onde o dado entra. Confie nos tipos dentro. Veja a skill de princípio **boundary-discipline**.

- **Formatos de fio** (proto, JSON-RPC). Faça parse com `ignoreUnknownFields` para que mudanças compatíveis com o futuro não quebrem clientes antigos.
- **JSON persistido.** Blob versionado com um try/catch em volta do parse.
- **Não revalide** no fundo das cadeias de chamada.

## Tipos derivados de schema

Quando um `.proto`, uma spec OpenAPI, um schema GraphQL ou uma migração de banco já define uma forma, derive dos tipos gerados em vez de duplicá-los.

```ts
// Não. Forma duplicada, divergem quando o schema muda.
type CheckSummary = {
  totalCount: number;
  checks: { name: string; status: string }[];
};
function renderChecks(s: CheckSummary) {
  /* ... */
}

// Faça. Derive do tipo de schema gerado.
import type { ChecksMessage } from "<generated module>";
function renderChecks(s: Pick<ChecksMessage, "totalCount" | "checks">) {
  /* ... */
}
```

Use `Pick`, `Omit`, `Parameters`, `ReturnType`, `Awaited`, `typeof` antes de escrever uma interface nova.

## Argumentos como objeto

```ts
// Não. Troque dois args, ainda compila.
openFile(uri, {
  startLineNumber: 10,
  startColumn: 1,
  endLineNumber: 10,
  endColumn: 1,
});

// Faça. Independente de ordem, autodocumentado.
openFile({
  uri,
  selection: {
    startLineNumber: 10,
    startColumn: 1,
    endLineNumber: 10,
    endColumn: 1,
  },
});
```

Pule em caminhos quentes. Render por frame, tokenizers, parsers, qualquer coisa num laço apertado onde o custo de alocação importa.
