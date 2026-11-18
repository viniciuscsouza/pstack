# pstack (pt-BR)

Tradução para português do Brasil do [pstack](https://github.com/cursor/plugins/tree/main/pstack), de [Lauren Tan](https://x.com/poteto), com quatro versões prontas para harnesses diferentes.

o pstack transforma o agente numa equipe de engenharia com processo. ir fundo antes de ir rápido, evidência antes de afirmação, unidades pequenas e verificadas antes de apostas grandes. o objetivo é escrever menos código, de mais qualidade.

## as quatro versões

| pasta | harness | o que instalar |
| --- | --- | --- |
| `claude-code/` | Claude Code | plugin com `.claude-plugin/` e marketplace |
| `opencode/` | OpenCode | `.opencode/` e `AGENTS.md` |
| `copilot/` | GitHub Copilot | `.github/` e `docs/` |
| `antigravity/` | Google Antigravity | `.agents/` e `AGENTS.md` |

Cada pasta é auto-contida. o conteúdo das skills é o mesmo nas quatro, porque os harnesses convergiram no padrão aberto Agent Skills, com uma pasta por skill contendo `SKILL.md`. O que muda por harness é a casca, e a casca está documentada em `docs/harness/<harness>.md` dentro de cada pasta: mapa de ferramentas, subagentes, modelos, paths e o que fazer com as features que só existem no Cursor.

## instalação rápida

Claude Code:

```bash
claude --plugin-dir /caminho/para/pstack/claude-code
```

OpenCode, Copilot e Antigravity: copie para a raiz do projeto o diretório de configuração da pasta correspondente, mais `AGENTS.md` ou `.github/`. O `README.md` de cada pasta traz o comando exato.

## o que tem dentro

- 58 skills, incluindo os 24 princípios, os 22 playbooks e o orquestrador `poteto-mode`
- 2 subagentes: `poteto-agent` e `comment-sicko`
- o guia do usuário em 10 capítulos, em `docs/guide/`

## o que não foi portado

- `automations/benny/`: depende de Cursor Automations, que não existe em nenhum dos quatro harnesses. O procedimento equivalente está descrito no mapa de cada harness, usando GitHub Actions, cron ou Dynamic Workflow.
- Código e fixtures: os scripts `.ts` das skills `statechart` e `poteto-mode`, os fixtures `.tla`, `.mmd`, `.dot`, `.out` e `.exit`, e os relatórios sob `scripts/test/corpus/` ficaram como estão. São executáveis e saída esperada de teste, não prosa. Traduzir os fixtures quebraria os testes que os comparam.

## modelos

Nenhuma skill fixa slug de modelo. Cada papel herda o modelo da sessão, e o mapa de cada harness explica como fixar um papel quando você quiser. Slug do Cursor não é copiado para harness nenhum, porque não existe fora dele.

## origem e licença

MIT, Copyright (c) 2026 Lauren Tan. O texto original está em [cursor/plugins](https://github.com/cursor/plugins), e a cópia sem tradução deste repositório está no branch `main`. Esta tradução vive no branch `pt-br`.

Os nomes das skills, os IDs, os caminhos e o código ficaram em inglês de propósito. Eles são referenciados entre dezenas de arquivos e viram comando de barra; traduzir quebraria as referências.
