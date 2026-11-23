# pstack no Claude Code

As skills do pstack usam nomes do Cursor. Neste harness, leia este arquivo uma vez por sessão e mapeie cada nome do Cursor com as tabelas abaixo. `<pstack>` é a pasta que contém `skills/` e `docs/`. Se um campo do Cursor não tem linha aqui, descarte e diga que descartou. Nunca passe um campo ou valor que o schema da ferramenta rejeita.

## Ferramentas

| Cursor | Claude Code |
| --- | --- |
| `Task` | `Agent` |
| `AskQuestion` | `AskUserQuestion`. Em modo `-p` ele não existe, então pergunte em texto puro. |
| Abrir uma todolist, ou copiar passos para ela | Use `TodoWrite` quando a sessão oferece. Sem ele, mantenha a lista na resposta e atualize a cada fase. |
| Rodar outra skill do pstack (`/nome` ou "a skill **nome**") | `Read` em `<pstack>/skills/<nome>/SKILL.md` e siga. O tool `Skill` recusa a maioria das skills do pstack. Para o usuário digitar, escreva `/pstack-pt-br:<nome>`. |
| `Grep`, `Glob` | `Bash` com `rg` quando a sessão não tem `Grep` ou `Glob`. |

## Campos de subagente

| Cursor | Claude Code |
| --- | --- |
| `subagent_type: generalPurpose` | `subagent_type: "general-purpose"` |
| `poteto-agent`, `comment-sicko` | `pstack-pt-br:poteto-agent`, `pstack-pt-br:comment-sicko` |
| A linha `model:` no arquivo do `poteto-agent` | Ela traz um slug do Cursor. Passe `model` e `effort` em toda chamada `Agent` que cria `pstack-pt-br:poteto-agent`, mapeados pela tabela de Modelos, para esse slug nunca chegar na API. |
| `readonly: true` | `subagent_type: "Explore"`. Ele mantém `Bash`, então o briefing também diz "não edite arquivos", e você confere `git status` depois. |
| `readonly: false` | `general-purpose`. Ele mantém os servidores MCP da sessão. |
| `environment: "cloud"` | `isolation: "worktree"`. Nomeie o branch ou commit que o worker faz checkout. |
| `environment: "local"` | Omita. |
| `cloud_base_branch: <b>` | Peça ao worker para fazer checkout de `<b>` primeiro. |
| `run_in_background: true` | Omita. Subagentes rodam em background por padrão e avisam quando terminam. |
| `resume` | Crie um agente novo e cole o resultado anterior no briefing dele. |
| `file_attachments` | Liste os caminhos no briefing. |

## Modelos

Padrão deste port: **herdar o modelo da sessão**. Só defina modelo explícito quando o usuário pedir.

| Cursor | Claude Code |
| --- | --- |
| `claude-<família>-<versão>-<esforço>[-fast]`, como `claude-opus-5-5-xhigh` | `model: "opus"` e `effort: "xhigh"`. `model` aceita só `opus`, `sonnet`, `haiku`, `fable`. Descarte `-fast`. |
| Slug `gemini-*` ou de outro fornecedor | Omita `model` para rodar no modelo da sessão, e diga que fez isso. |
| `auto` ou `inherit-parent` | Omita `model`. |
| A regra `pstack-models.mdc`, `~/.cursor/rules/pstack-models.mdc` | `~/.claude/rules/pstack-models.md`. O Claude Code carrega essa regra em toda sessão nova, então as linhas de papel já estão em contexto. Se não estiverem, leia o arquivo. Se o arquivo ou a linha do papel faltar, use o padrão da skill. |
| Um valor de papel `<modelo> <esforço>`, como `opus high` | `model: "opus"` e `effort: "high"`. |

## setup-pstack

O Claude Code carrega todo `.md` de `~/.claude/rules/` em cada sessão nova, então o `setup-pstack` escreve a regra ali. Siga os passos dele com estas mudanças.

- Detectar. Os modelos detectados são os valores de `model` que o schema do tool `Agent` lista, cada um com um esforço dos valores de `effort` dele. Escreva um modelo real como `<modelo> <esforço>`, como `opus xhigh`. O esforço é o último token, então o passo de orçamento funciona como está escrito.
- Padrões. Mapeie cada slug padrão pelas linhas acima. `claude-opus-5-5-xhigh-fast` vira `opus xhigh`, `claude-opus-5-5-high-fast` vira `opus high`, `claude-fable-5-2-high` vira `fable high`, e `gemini-4-carbon-high` vira `inherit-parent`.
- Escrever. Grave `~/.claude/rules/pstack-models.md` sem frontmatter, depois as linhas de comentário, a linha `# budget`, e uma linha por papel do passo 5. Sobrescreva o arquivo inteiro. Nunca ponha as linhas de papel em `CLAUDE.md` ou `AGENTS.md`.

## Paths

| Cursor | Claude Code |
| --- | --- |
| `.cursor/skills/`, `~/.cursor/skills/` | `.claude/skills/`, `~/.claude/skills/` |
| `~/.cursor/plugins/`, `.cursor-plugin/plugin.json` | `~/.claude/plugins/`, `.claude-plugin/plugin.json` |
| `.cursor/rules/*.mdc`, regras sempre aplicadas | `CLAUDE.md`, ou `AGENTS.md` quando não existe `CLAUDE.md`. O Claude Code nunca lê `.mdc`. A regra de modelo é a exceção. Veja Modelos. |
| `https://github.com/cursor/plugins/blob/main/pstack/<path>` | `<pstack>/<path>` quando o arquivo existe. O plugin entrega as skills, `docs/guide/` e `README.md`. Leia a cópia local e nunca busque na web. Dê a URL ao usuário quando ele precisar de um link. |
| Transcrições em `~/.cursor/projects/<slug>/agent-transcripts/` | `~/.claude/projects/<slug>/<sessão>.jsonl`. O slug é o caminho do workspace com cada `/` e `.` trocado por `-`, então `/home/voce/proj` vira `-home-voce-proj`. O `.jsonl` mais novo direto nessa pasta é a sessão atual. A pasta `<sessão>/subagents/` guarda as transcrições dos subagentes. Leia só a pasta deste workspace. |

## Features exclusivas do Cursor

O Claude Code não tem Automations, Bugbot, agentes na nuvem, nem tool de PR embutido. Cada uma tem substituto, e o substituto é o padrão. Só escreva uma linha `Pulado no Claude Code: <passo>. <motivo>.` quando não houver nenhum equivalente e o passo não puder ser cumprido de outro jeito.

| Cursor | Claude Code |
| --- | --- |
| Cursor Automations: rotinas, chamadas `update_state`, webhooks `api2.cursor.sh/automations`, despertar de rotina, e pedidos de segredo `SendToUser` | GitHub Actions rodando `claude -p` em modo headless, ou um script com `cron`. Grave o prompt da rotina num arquivo e chame esse arquivo no job. Segredo vem de `secrets.` do repositório, nunca de um pedido em chat. |
| Bugbot | Copilot code review ou um job de review no CI. Comentários de review em PR do GitHub são threads comuns, então faça a triagem com `gh pr view --comments` e `gh api`. O passo que configuraria o Bugbot em si não tem equivalente, e vira `Pulado no Claude Code: ...`. |
| Agentes na nuvem do Cursor | `isolation: "worktree"` mais um branch nomeado, como na tabela de subagentes. Sem painel de nuvem: o estado do worker é o branch e o diff. |
| Tool de PR embutido | `gh pr create`, `gh pr view`, `gh pr checks`. Isso não é pulo, então não escreva linha nenhuma. |
| `/deslop` do plugin `cursor-team-kit` | A skill **unslop** deste plugin. |
| `control-ui` / `control-cli` do `cursor-team-kit` | Teste a UI dirija o app real pelo playbook **Visual parity**, e a CLI pelo comando que o repositório já tiver. Não invente harness de teste. |
