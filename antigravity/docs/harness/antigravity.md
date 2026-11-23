# pstack no Antigravity

As skills do pstack usam nomes do Cursor. Aqui, leia este arquivo uma vez por sessão e mapeie cada nome com as tabelas abaixo. `<pstack>` é a pasta que contém `skills/` e `docs/`. Campo sem linha aqui não existe neste harness, então descarte sem inventar equivalente.

## Onde as coisas ficam

| Peça | Caminho no workspace | Caminho global |
| --- | --- | --- |
| Skills | `.agents/skills/<nome>/SKILL.md` | `~/.gemini/config/skills/<nome>/` |
| Agentes | `.agents/agents/<nome>.md` | `~/.gemini/config/agents/<nome>.md` |
| Regras | `.agents/rules/*.md`, `AGENTS.md`, `GEMINI.md` | `~/.gemini/AGENTS.md`, `~/.gemini/GEMINI.md` |
| Plugins | `.agents/plugins/<pasta>/` | `~/.gemini/config/plugins/` |

O Antigravity lê `.agents/` em todas as pastas acima do projeto até a raiz do repositório. Regras são cumulativas, não há substituição.

## Ferramentas

| Cursor | Antigravity |
| --- | --- |
| `Read`, `Grep`, `Glob` | Leitura de arquivo e busca do agente. |
| `Bash` | Terminal do agente. |
| `AskQuestion` | Sem pergunta interativa. Pergunte em texto puro e siga quando a decisão for reversível. |
| `Task` | Agente personalizado em `.agents/agents/<nome>.md`, invocado pelo agente principal. |
| Todolist | Sem tool de lista. Mantenha a lista na resposta. |
| Rodar outra skill do pstack (`/nome`) | Barra com o nome da skill, como `/unslop`. O agente também carrega sozinho quando a `description` casa com a tarefa. |

## Agentes

| Cursor | Antigravity |
| --- | --- |
| `subagent_type: generalPurpose` | O agente principal, sem delegação, quando não houver agente especializado. |
| `poteto-agent`, `comment-sicko` | `.agents/agents/poteto-agent.md` e `.agents/agents/comment-sicko.md` |
| `readonly: true` | Um agente cujo corpo e regras proíbem edição. Não há campo de permissão por agente aqui, então a proibição é instrução, e você confere `git status` depois. |
| `environment: "cloud"` | Não existe. Use `git worktree add` e nomeie o branch. |
| `run_in_background: true` | Omita. |
| `file_attachments` | Liste os caminhos no briefing. |

Para trabalho que precisa de várias fases em paralelo, o Antigravity tem **Dynamic Workflows**: o agente escreve e roda um script Python que orquestra vários agentes. É o substituto mais próximo de um leque de subagentes do Cursor, e é o que usar quando o playbook pede fan-out.

## Modelos

Padrão deste port: **herdar o modelo da sessão**. O frontmatter de skill aqui aceita só `name` e `description`, então não existe campo de modelo por skill. Modelo se escolhe na sessão.

| Cursor | Antigravity |
| --- | --- |
| `claude-<família>-<versão>-<esforço>[-fast]` | Escolha no seletor da sessão. Não escreva slug no arquivo. |
| Slug `gemini-*` | `gemini-*` no seletor, quando disponível. |
| `auto` ou `inherit-parent` | Omita qualquer escolha. |
| A regra `pstack-models.mdc` | `.agents/rules/pstack-models.md`, como regra do workspace, com `trigger: always_on` no frontmatter. |

## setup-pstack

Limite conferido em 2026-11-23 na doc oficial do Antigravity: 24.000 bytes por arquivo de regra, após expandir includes, e orçamento agregado de 20.000 tokens para regras globais e `always_on`. Se a tabela de papéis passar do limite por arquivo, quebre em dois arquivos, um com os papéis de código e outro com prosa e julgamento.

- Detectar. Liste os modelos do seletor. Não copie slug do Cursor.
- Padrões. Sem escolha do usuário, cada papel herda a sessão.
- Escrever. Grave `.agents/rules/pstack-models.md` com frontmatter `trigger: always_on` e `description: "...". Todo `.md` em `rules/` sem `trigger` válido é descartado silenciosamente pelo Antigravity. Não repita as linhas em `AGENTS.md`, porque as duas cópias vão divergir.

## Paths

| Cursor | Antigravity |
| --- | --- |
| `.cursor/skills/`, `~/.cursor/skills/` | `.agents/skills/`, `~/.gemini/config/skills/` |
| `~/.cursor/plugins/`, `.cursor-plugin/plugin.json` | `.agents/plugins/<pasta>/`. Sem manifest próprio: a pasta é o plugin. |
| `.cursor/rules/*.mdc`, regras sempre aplicadas | `.agents/rules/*.md`, `AGENTS.md` ou `GEMINI.md`. O Antigravity não lê `.mdc`. |
| Workflows em `.cursor/workflows/` | Skill, não workflow. Workflow é legado e será descontinuado. |
| `https://github.com/cursor/plugins/blob/main/pstack/<path>` | `<pstack>/<path>` na cópia local. |
| Transcrições em `~/.cursor/projects/<slug>/agent-transcripts/` | Painel de sessões do app. Não dependa de caminho fixo no sistema de arquivos. |

## Features exclusivas do Cursor

| Cursor | Antigravity |
| --- | --- |
| Cursor Automations: rotinas, `update_state`, webhooks, `SendToUser` | Dynamic Workflow agendado, ou um script `cron` chamando o Antigravity CLI. Segredo vem de variável de ambiente. |
| Bugbot | Não há review de bot nativo. Use `gh pr view --comments` mais a triagem do `references/bugbot-triage.md`, que continua válida como procedimento. |
| Agentes na nuvem | `git worktree add` mais um branch nomeado. |
| Tool de PR embutido | `gh pr create`, `gh pr view`, `gh pr checks`. |
| `/deslop` do `cursor-team-kit` | A skill **unslop**. |
| `control-ui` / `control-cli` | Playbook **Visual parity** para UI, comando do repositório para CLI. |

Escreva `Pulado no Antigravity: <passo>. <motivo>.` só quando o passo configura uma peça do Cursor que não tem equivalente nenhum, como o painel de Automations ou as configurações do Bugbot.
