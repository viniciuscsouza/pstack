# pstack no GitHub Copilot

As skills do pstack usam nomes do Cursor. Aqui, leia este arquivo uma vez por sessão e mapeie cada nome com as tabelas abaixo. `<pstack>` é a pasta que contém `skills/` e `docs/`. Campo sem linha aqui não existe neste harness, então descarte em vez de inventar equivalente.

## Onde as coisas ficam

| Peça | Caminho |
| --- | --- |
| Skills | `.github/skills/<nome>/SKILL.md` |
| Agentes | `.github/agents/<nome>.md` |
| Instruções sempre ativas | `.github/copilot-instructions.md` |
| Comandos de barra | `.github/prompts/<nome>.prompt.md` |
| Instruções por caminho | `.github/instructions/*.instructions.md` |

O Copilot também lê skills de `.claude/skills/` e `.agents/skills/`, mas este pacote usa `.github/` para ser explícito.

## Ferramentas

| Cursor | GitHub Copilot |
| --- | --- |
| `Read`, `Grep`, `Glob` | Leitura e busca de arquivo do agente. No Copilot CLI, o tool de leitura de arquivo e o de busca. |
| `Bash` | Shell do Copilot CLI. No app e nos IDEs, o tool de execução de terminal. |
| `AskQuestion` | Sem pergunta interativa no CLI. Pergunte em texto puro e siga quando a decisão for reversível. |
| `Task` | Agente embutido: `explore`, `task`, `general-purpose`, `code-review`, `research`, `rubber-duck` ou `security-review`. |
| Todolist | Sem tool de lista. Mantenha a lista na resposta e atualize a cada fase. |
| Rodar outra skill do pstack (`/nome`) | Leia `.github/skills/<nome>/SKILL.md` e siga. Se existir um prompt file com o mesmo nome, o usuário pode chamar por barra. |

## Agentes

| Cursor | GitHub Copilot |
| --- | --- |
| `subagent_type: generalPurpose` | `general-purpose` |
| `poteto-agent`, `comment-sicko` | `poteto-agent`, `comment-sicko` em `.github/agents/` |
| `readonly: true` | `explore`, que lê e busca sem editar |
| Revisão de diff | `code-review` |
| Segunda opinião | `rubber-duck` |
| Revisão de segurança | `security-review` |
| `environment: "cloud"` | O Copilot cloud agent do próprio GitHub, ou um worktree quando você quiser controle local |
| `run_in_background: true` | Omita. Delegação já roda em contexto separado. |
| `file_attachments` | Liste os caminhos no briefing. |

O campo `tools` do agente personalizado restringe o que ele pode usar. Para um agente de leitura, liste só as ferramentas de leitura em vez de confiar no briefing.

## Modelos

Padrão deste port: **herdar o modelo da sessão**. O campo `model` do agente é opcional.

| Cursor | GitHub Copilot |
| --- | --- |
| `claude-<família>-<versão>-<esforço>[-fast]` | `model` do agente, com o identificador que o ambiente aceita, mais `reasoningEffort` quando ele oferecer. Confira no seletor de modelo da sessão antes de escrever. |
| Slug `gemini-*` ou de outro fornecedor | Omita `model` e deixe o ambiente resolver. |
| `auto` ou `inherit-parent` | Omita `model`. |
| A regra `pstack-models.mdc` | `.github/copilot-instructions.md`, que é carregado sempre. As linhas de papel ficam lá. |

## setup-pstack

- Detectar. Liste os modelos que o seletor da sessão oferece. Não copie slug do Cursor.
- Padrões. Sem escolha do usuário, cada papel herda a sessão, ou seja, sem `model` no agente.
- Escrever. Grave a tabela de papéis em `.github/copilot-instructions.md`, numa seção `## pstack: papéis e modelos`. Sobrescreva só essa seção e não duplique em outro arquivo.

## Paths

| Cursor | GitHub Copilot |
| --- | --- |
| `.cursor/skills/`, `~/.cursor/skills/` | `.github/skills/`, `~/.copilot/skills/` |
| `~/.cursor/plugins/`, `.cursor-plugin/plugin.json` | Sem plugin manifest. A instalação é copiar `.github/` para o repositório. |
| `.cursor/rules/*.mdc` | `.github/copilot-instructions.md` para o repositório, `.github/instructions/*.instructions.md` para caminhos específicos. |
| `https://github.com/cursor/plugins/blob/main/pstack/<path>` | `<pstack>/<path>` na cópia local. |
| Transcrições em `~/.cursor/projects/<slug>/agent-transcripts/` | Sessões do Copilot CLI no diretório de estado local do CLI. Não dependa de caminho fixo: peça ao usuário o id da sessão. |

## Features exclusivas do Cursor

Aqui está a vantagem deste harness. Duas das features exclusivas do Cursor existem nativamente no GitHub, e não há nada a pular.

| Cursor | GitHub Copilot |
| --- | --- |
| Cursor Automations: rotinas, `update_state`, webhooks, `SendToUser` | GitHub Actions com o Copilot cloud agent, ou `schedule:` chamando o Copilot CLI. Segredo vem de `secrets.` do repositório. |
| Bugbot | Copilot code review no PR. É nativo: ative o review automático no repositório e trate os comentários como threads normais com `gh pr view --comments`. |
| Agentes na nuvem | Copilot cloud agent atribuído pela issue ou pelo PR. O estado do worker é a sessão do agente e o branch que ele abriu. |
| Tool de PR embutido | `gh pr create`, `gh pr view`, `gh pr checks`. |
| `/deslop` do `cursor-team-kit` | A skill **unslop**. |
| `control-ui` / `control-cli` | Playbook **Visual parity** para UI, comando do repositório para CLI. |

Só escreva `Pulado no GitHub Copilot: <passo>. <motivo>.` quando o passo configurar uma peça do Cursor em si, como o painel de Automations ou as configurações do Bugbot.
