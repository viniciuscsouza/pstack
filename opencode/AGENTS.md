# pstack

Este projeto usa o pstack. As skills ficam em `.opencode/skills/` e carregam sozinhas quando a `description` casa com a tarefa.

## Como usar

- `@poteto-mode` ou pedir para trabalhar no estilo do poteto liga o modo completo. O `poteto-mode` é o orquestrador: ele escolhe o playbook, mantém a lista de tarefas e delega para o `poteto-agent`.
- Uma skill isolada roda com `@nome`, ou pelo tool `skill` com o ID exato, como `skill({ name: "unslop" })`.
- Antes do primeiro passo que nomeia uma ferramenta do Cursor, leia `docs/harness/opencode.md` uma vez por sessão. Ele traz o mapa de ferramentas, subagentes, modelos e paths deste harness, e o que fazer com as features que só existem no Cursor.

## Modelos

Cada papel herda o modelo da sessão por padrão. Se quiser fixar um papel, escreva `model: "<provedor>/<modelo>#<esforço>"` no arquivo do agente e confirme o ID com `opencode models`. Não copie slug do Cursor, porque eles não existem aqui.

O modelo explícito por papel do harness do Cursor vive em `.cursor/rules/pstack-models.mdc`. Aqui, a tabela de papéis, quando existir, vive neste arquivo, na seção `## pstack: papéis e modelos`.

## Regras que valem sempre

- Nomeie a forma dos dados antes de escrever lógica.
- Verifique contra o artefato real, nunca contra um proxy ou um "compila".
- Corrija a causa raiz. Uma guarda só é correção quando o estado que ela trata é legítimo.
- Não entregue ao humano uma checagem que você poderia rodar.
