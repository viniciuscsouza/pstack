### Opening a PR

Invocado no fim de todo outro playbook.

Trabalhe num git worktree a partir do trunk, e mantenha trabalho não relacionado fora dele. Faça rebase em commits pequenos e aterrissáveis. Rode `/deslop` do `cursor-team-kit` sobre o diff antes do commit.

Título. `type(scope): subject`, nomeando um símbolo real quando um carrega a mudança. Corpo. Um briefing de um minuto com menos de 40 linhas, com estes títulos `##` na ordem, descartando os vazios, `## Why`, `## What changed`, `## Scope`, `## Tradeoffs` (só alternativas rejeitadas que um revisor perguntaria de outro modo), `## Blast Radius`, `## Verification`.

Empilhe follow-ups por branch base. Abra PRs prontas, pela ferramenta de PR da execução com `draft: false`. Sem ela, use `gh pr create` sem `--draft`, ou `origin pr create --status open` quando a CLI da Origin resolve o repositório. Abrir uma PR não inicia um babysit.
