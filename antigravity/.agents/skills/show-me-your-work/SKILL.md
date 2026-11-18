---
name: show-me-your-work
description: "Mantenha uma trilha de decisões revisável para trabalho longo ou desassistido: um log TSV com uma linha por decisão (o quê, por quê, evidência, resultado). Local por padrão; commite quando um revisor precisa da trilha para confiar no resultado. Use para /show-me-your-work, execuções autônomas ou multifásicas, ou trabalho que um humano revisa depois de se ausentar."
---

# Mostre o seu trabalho

Mantenha um único log canônico.

## O formato

Um único arquivo TSV, uma linha por decisão. As células ficam em uma linha só. Evidência é um ponteiro, não prosa.

Copie `references/decision-log-template.tsv` (a linha de cabeçalho) para começar um log limpo. Colunas:

- **ts.** Timestamp ISO8601.
- **phase.** A fase ou frente de trabalho.
- **decision.** O que foi escolhido ou feito, em uma linha.
- **why.** O motivo em palavras simples. Se um princípio guiou, diga isso claramente, não como etiqueta de jargão.
- **evidence.** Um link ou caminho que prova: SHA do commit, número da PR, `file:line`, ou o caminho de um artefato, trace ou screenshot. Nunca um parágrafo.
- **result.** O desfecho ou estado do predicado: `tests green`, `reverted`, `pixel-diff 0`, `INCONCLUSIVE`, `open`.

Um exemplo, em linguagem simples para um revisor ler de relance.

```
ts	phase	decision	why	evidence	result
2026-05-24T09:02:00Z	frame	counted the work first, about 100 components and roughly 75 hours	wanted to know the size before starting a long run	commit 3a9f1c2	found 5 things to sort out before starting
2026-05-24T09:40:00Z	harness	took screenshots of the old version before changing anything	so we can compare old against new and catch any visual change	scripts/snapshot.sh, baseline/	saved 120 reference screenshots
2026-05-24T11:15:00Z	widget	moved the widget styles over without changing how it looks	keep the change small and the result identical	commit 7c21e0a, pixel-diff 0	looks identical, tests pass
2026-05-24T12:30:00Z	widget	threw out a helper's work because its screenshots were blank	checked the real files instead of trusting its summary	worktree reset	reverted, tightened the instructions for next time
```

## Registrando uma linha

Escreva cada entrada como você contaria a um colega o que fez. Palavras simples, ações concretas, sem fala de IA nem jargão abstrato (a skill **unslop** vale também para o texto do log).

Use o helper `scripts/log.sh <logfile> <phase> <decision> <why> <evidence> <result>`. Ele carimba `ts`, escreve o cabeçalho no primeiro uso, remove tabs e quebras de linha perdidas, e prefixa com aspas simples qualquer célula que comece com `=`, `+`, `-` ou `@`. Um `printf` simples acrescentando uma linha também funciona, mas cuide desses mesmos bytes se as células vierem de texto gerado ou fornecido pelo usuário.

Registre pontos de decisão e checkpoints, não cada ação: uma bifurcação escolhida, uma unidade concluída com o resultado da verificação, um pivô ou reversão com o gatilho, um bloqueio trazido à tona, um portão consertado. Para execuções em laço, uma linha por iteração. Pule o trivial e o óbvio.

Uma execução é uma conversa de agente, incluindo as rodadas posteriores dela e qualquer resumo dela. Um pickup, um agente substituto, ou um chat novo iniciam uma execução nova. Quando uma execução acrescenta a um log que já tem linhas, a primeira linha dela tem fase `start`, e também a primeira linha dela depois da linha `start` de outra execução. Então uma execução que volta a um log numa rodada posterior primeiro lê as últimas linhas do log para ver se outra execução escreveu desde então. Uma linha `start` nomeia o intervalo de `ts` das linhas antes dela que esta execução não escreveu, e a evidência dela nomeia esta execução, como o id do agente. Use a fase `start` para nada mais.

## Onde ele fica

Por padrão o log é um artefato de trabalho, não commitado. Mantenha em `decisions.tsv` no diretório de trabalho, ou `.audit/<task-slug>.tsv` quando vários esforços rodam ao mesmo tempo, e deixe fora do git.

Commite só quando o trabalho é ambicioso o bastante para um revisor precisar da trilha para confiar no resultado.

## Regras

- Só acrescentar. Uma chamada errada ganha uma linha nova que a substitui. Nunca edite nem apague histórico.
- Prefira evidência produzida por scripts commitados a trabalhos manuais avulsos (a skill de princípio **encode-lessons-in-structure**).

## Audite o log contra a transcrição

No fim da execução, antes de devolver, verifique se o log disse a verdade. Leia a transcrição desta execução no diretório `agent-transcripts/` do workspace ativo (o system prompt nomeia o caminho). Não faça glob por `~/.cursor/projects/*/`. Isso lê chats privados não relacionados. Percorra as linhas desta execução contra o que de fato aconteceu. Cada trecho delas começa numa das linhas `start` desta execução, ou na primeira linha se esta execução criou o log, e termina na próxima linha `start` de outra execução:

- Verifique que cada linha mapeia para uma decisão ou ação real.
- Verifique que a evidência de cada linha resolve e mostra o que a linha afirma.
- Uma bifurcação, pivô ou abordagem abandonada que moldou o trabalho mas não está no log é uma lacuna. Acrescente.

Corrija o log, não a história. A auditoria nunca edita nem remove uma linha, nem uma inventada. Quando uma linha não registra nem uma decisão nem uma ação real, ou a afirmação ou a evidência dela está errada, acrescente uma linha que a substitui com o que de fato aconteceu e um ponteiro que resolve. Esta auditoria não checa linhas fora dos trechos desta execução. Se o trabalho desta execução mostra que uma delas está errada, substitua como qualquer chamada errada.

## Revisão da trilha por outro modelo

Antes de devolver, crie um subagente no modelo de outro fornecedor, diferente do que fez o trabalho. Autorrevisão não substitui. O subagente lê a trilha de auditoria e a transcrição da execução, depois aponta o que o usuário deve observar. Não é refazer o trabalho, é uma varredura do que está abaixo do ideal ou arriscado. Escolha o nível de raciocínio de cada subagente para o trabalho, conforme a seção Subagentes da skill **poteto-mode**.

- Decisões registradas com evidência fraca ou ausente.
- Passos de verificação pulados ou afirmados sem prova na transcrição.
- Escolhas que parecem arriscadas em retrospecto (prematuras, que inflaram o escopo, que encobriram um sintoma).
- Lacunas que o usuário perderia numa olhada casual.

Toda resposta de uma execução que produziu uma trilha termina com uma seção "Attention". Comece com o modelo do revisor em linha própria (`reviewed by <model>`), depois liste cada apontamento indicando linhas ou momentos específicos. "No flags" é um valor válido. O nome do modelo não é.

## Revisando a trilha

Leia de cima a baixo, siga os ponteiros de evidência, confira por amostragem. O GitHub renderiza um TSV commitado como tabela. `column -s$'\t' -t decisions.tsv` renderiza no terminal.

## Compondo esta skill

Outras skills roteiam a trilha de auditoria delas para cá em vez de inventar uma. Referencie pelo nome e deixe que ela seja dona do formato. Não repita as colunas.
