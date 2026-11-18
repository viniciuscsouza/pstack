# pstack no OpenCode

As skills do pstack usam nomes do Cursor. Aqui, leia este arquivo uma vez por sessão e mapeie cada nome com as tabelas abaixo. `<pstack>` é a pasta que contém `skills/` e `docs/`. Campo sem linha aqui é campo que não existe no OpenCode, então descarte sem inventar equivalente.

## Ferramentas

| Cursor | OpenCode |
| --- | --- |
| `Read`, `Grep`, `Glob`, `Write`, `Edit` | `read`, `glob`, `grep`, `write`, `edit`, `patch`. Tudo em minúsculas. |
| `Bash` | `shell`. Use `workdir` em vez de `cd` no comando, e `timeout` em milissegundos. |
| `WebFetch`, busca web | `webfetch`, `websearch`. |
| `Task` | `subagent`, com o id do agente. Ele sempre roda em background e devolve `sessionID`. |
| `AskQuestion` | Não existe pergunta interativa. Pergunte em texto puro na resposta e siga quando a decisão for reversível. |
| Todolist | Não existe tool de lista. Mantenha a lista na resposta, atualize a cada fase, e copie os passos do playbook para ela antes de começar. |
| Rodar outra skill do pstack (`/nome`) | O tool `skill` com o ID exato, como `skill({ name: "unslop" })`. O IDs são os mesmos do Cursor, sem tradução. O usuário também pode escrever `@nome` no prompt. |
| `subagent_type: poteto-agent` | O agente `poteto-agent` em `.opencode/agents/`. |

## Subagentes

| Cursor | OpenCode |
| --- | --- |
| `subagent_type: generalPurpose` | Agente embutido `general` ou o agente de build da sessão. Confira a lista de agentes disponíveis antes de nomear um. |
| `poteto-agent`, `comment-sicko` | `poteto-agent`, `comment-sicko` |
| `readonly: true` | Um agente com `permissions` negando `edit` e `shell`, ou o subagente `explore` quando existir. O briefing também diz "não edite arquivos". |
| `environment: "cloud"` | Não existe. Use um worktree do git com `git worktree add`, e nomeie o branch no briefing. |
| `run_in_background: true` | Omita. Todo subagente já roda em background e notifica a sessão quando fica ocioso. |
| `resume` | O `sessionID` que o `subagent` devolveu. Retome só quando o trabalho precisa de estado que vive ali. |
| `file_attachments` | Liste os caminhos no briefing. O subagente lê com `read`. |

## Modelos

Padrão deste port: **herdar o modelo da sessão**. O campo `model` do agente é opcional, e omitir é o que garante herança.

| Cursor | OpenCode |
| --- | --- |
| `claude-<família>-<versão>-<esforço>[-fast]` | `provider/modelo#esforço`, como `anthropic/claude-sonnet-5-5#high`. Confirme o id real com `opencode models` antes de escrever. |
| Slug `gemini-*` | `provider/modelo` do provedor configurado, se ele estiver autenticado. Senão omita e diga que herdou. |
| `auto` ou `inherit-parent` | Omita `model`. |
| A regra `pstack-models.mdc` | `AGENTS.md` na raiz do projeto, ou `~/.config/opencode/AGENTS.md`. O OpenCode carrega esse arquivo em toda sessão, então as linhas de papel já estão em contexto. |
| Um valor de papel `<modelo> <esforço>` | `model: "<provider>/<modelo>#<esforço>"` no arquivo do agente. |

## setup-pstack

O `setup-pstack` foi escrito para escrever uma regra em `~/.cursor/rules/`. No OpenCode o equivalente é `AGENTS.md`, e ele é carregado inteiro em toda sessão.

- Detectar. Rode `opencode models` e use os IDs reais. Não copie slug do Cursor.
- Padrões. Se o usuário não escolheu, escreva `inherit-parent` em cada papel.
- Escrever. Grave um bloco em `AGENTS.md` do projeto com as linhas de papel, dentro de uma seção `## pstack: papéis e modelos`. Sobrescreva só essa seção. Nunca duplique o bloco em dois arquivos, porque as duas cópias vão divergir.

## Paths

| Cursor | OpenCode |
| --- | --- |
| `.cursor/skills/`, `~/.cursor/skills/` | `.opencode/skills/`, `~/.config/opencode/skills/`. O OpenCode também lê `.claude/skills/` e `.agents/skills/`, nesta ordem de prioridade. |
| `~/.cursor/plugins/`, `.cursor-plugin/plugin.json` | Não existe plugin manifest. A pasta `.opencode/` do projeto é a instalação. |
| `.cursor/rules/*.mdc` | `AGENTS.md`, carregado sempre. O OpenCode não lê `.mdc`. |
| `https://github.com/cursor/plugins/blob/main/pstack/<path>` | `<pstack>/<path>` na cópia local. Nunca busque na web o que já está no disco. |
| Transcrições em `~/.cursor/projects/<slug>/agent-transcripts/` | Armazenamento local de sessões do OpenCode. Não dependa de um caminho fixo: peça ao usuário o `sessionID` ou a sessão, e leia o que o tool expõe. |

## Features exclusivas do Cursor

Nenhuma delas existe no OpenCode, então nenhuma pode simplesmente desaparecer. Cada linha diz o substituto.

| Cursor | OpenCode |
| --- | --- |
| Cursor Automations: rotinas, `update_state`, webhooks `api2.cursor.sh`, despertar de rotina, `SendToUser` | Um script `cron` chamando `opencode run` com o prompt da rotina gravado num arquivo, ou um job de GitHub Actions. Segredo vem de variável de ambiente, nunca de chat. O passo que configura a rotina em si vira a linha `Pulado no OpenCode: ...`. |
| Bugbot | `gh pr view --comments` mais triagem própria, ou um job de CI. O passo que configura o Bugbot vira linha de pulo. |
| Agentes na nuvem | `git worktree add`. O branch é o estado do worker. |
| Tool de PR embutido | `gh pr create`, `gh pr view`, `gh pr checks`. |
| `/deslop` do `cursor-team-kit` | A skill **unslop**. |
| `control-ui` / `control-cli` | O playbook **Visual parity** para UI. Para CLI, o comando que o repositório já tem. |
