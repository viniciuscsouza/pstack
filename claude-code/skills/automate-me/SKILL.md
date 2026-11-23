---
name: automate-me
description: "Use para \"automate me\", \"create/update/refresh my -mode skill\", \"turn/capture my preferences or working style into a skill\", ou para querer que agentes sigam como o usuário trabalha. Rascunha ou revisa uma skill pessoal -mode via create-skill + unslop, opcionalmente puxando evidência nova de transcripts recentes."
disable-model-invocation: true
---

# Automatize-me

Um fluxo guiado que transforma as convenções de trabalho do usuário numa skill que agentes seguem. A saída é uma skill `-mode` sob medida para ele (ex. `jay-mode`, `priya-mode`).

Esta skill orquestra outras três, uma passada de mineração inline (veja o passo 1), o `create-skill` nativo do Cursor (autoria) e a skill **unslop** (disciplina de prosa). Ela sequencia as três. Não substitui nenhuma.

## Fluxo

### 0. Verifique se já existe uma skill

Procure recursivamente por `.cursor/skills/**/*-mode/SKILL.md` e `~/.cursor/skills/*-mode/SKILL.md` que casem com o handle do usuário. Skills de modo podem viver num diretório de categoria pessoal (`.cursor/skills/<handle>/`), não só no nível de cima. Se uma existir, confirme a intenção com `AskQuestion` (a menos que a pessoa já tenha dito "update my skill" ou algo parecido):

- Atualizar a skill existente (padrão em execuções repetidas)
- Começar do zero (raro, pergunte por que antes de fazer)

O modo de atualização muda o resto do fluxo:
- O passo 1 minera só o histórico desde a última edição da skill (`git log -1 --format=%cI <path>`).
- O passo 2 pergunta o que mudou ou falta, não o que capturar do zero.
- O passo 4 edita o arquivo existente no lugar. Preserve seções que o usuário não contradisse. Revise as que têm evidência nova. Adicione seções novas só para regras de fato novas.

### 1. Minere o histórico

Localize os transcripts do workspace ativo antes do leque. O system prompt nomeia o diretório `agent-transcripts/` do workspace. Use só esse caminho. Não faça glob por `~/.cursor/projects/*/`. Isso cruza fronteiras de workspace e lê chats privados de projetos alheios.

Varra as conversas recentes de agente nesse escopo atrás de padrões recorrentes. Rode vários subagentes em paralelo sobre fatias do histórico (ex. as últimas 2 a 4 semanas em 3 fatias, cada uma com material suficiente). Escolha o nível de raciocínio de cada subagente conforme a seção Subagentes da skill **poteto-mode**. Cada subagente de mineração lê os transcripts do caminho com escopo de workspace que o pai fornece, procura os sinais abaixo, e devolve uma lista estruturada curta dos padrões que viu com ponteiros de evidência. Sinais padrão:

- Preferências de resposta (tamanho, tom, formato, correções de "dumb it down")
- Hábitos de delegação (subagentes, modelos, fluxos especializados, paralelismo)
- Postura de verificação (o que "done" significa, testes unitários vs repro ao vivo, revisores)
- Disciplina de código e prosa (estilo, princípios citados, ferramentas de lint/formatação)
- Convenções de processo (worktrees, commits, PRs, ferramentas de revisão/merge)
- Preferências meta (corrigir skills no meio da tarefa, propor skills novas)

Cruze as fatias antes de elevar um sinal. Padrões vistos em 2 ou mais fatias têm alta confiança. Sinais isolados são fracos e em geral caem.

### 2. Pergunte direto ao usuário

A mineração perde intenção que ainda não apareceu. Use a ferramenta `AskQuestion` (múltipla escolha estruturada) em vez de pedir para o usuário digitar do zero.

Formato. Uma ou duas perguntas com 4 a 6 opções cada, `allow_multiple: true` para perguntas de categoria. Comece amplo ("Que áreas importam mais?"), depois aprofunde nas áreas escolhidas com opções específicas. Depois das rodadas estruturadas, uma pergunta livre no chat pega o que as opções perderam.

Não despeje 20 perguntas.

### 3. Agrupe os achados

Agrupe os sinais combinados em seções. As comuns (use só o que se aplica):

- **Estilo de resposta**. Tamanho, tom, formato.
- **Autonomia**. Quanto fazer sem perguntar, uso de ferramentas MCP.
- **Entenda primeiro**. Quais skills acionar ao delimitar ou investigar uma mudança.
- **Subagentes**. Padrão, paralelismo, modelo por tarefa, fluxos especializados.
- **Disciplina de prosa / código**. Princípios, ferramentas de lint, guias de estilo.
- **Revisão e verificação**. Postura de repro, skills de verificação, ferramentas de teste ao vivo.
- **Processo**. git worktrees, commits, PRs, ferramentas de revisão/merge.
- **Skills**. Hábitos de autoria de skill, corrigir a skill primeiro, propor skills novas.

A skill **poteto-mode** mostra a forma. Leia para calibrar a granularidade. Não copie o conteúdo. As regras do usuário não são as do poteto-mode.

### 4. Rascunhe a skill

Use a skill `create-skill` nativa do Cursor para escrever a skill. Onde colocar:

- Caminho. Preserve a categoria de uma skill de modo existente. Para um modo novo, use `.cursor/skills/<handle>/<handle>-mode/SKILL.md` quando o repositório tem uma categoria pessoal estabelecida para aquele handle. Caso contrário, o padrão é `.cursor/skills/<handle>-mode/SKILL.md` no projeto (ou `~/.cursor/skills/<handle>-mode/` se o usuário preferir uma skill pessoal).
- Handle. O primeiro nome do usuário ou o identificador que ele escolher.
- `description` do frontmatter. Dispare no nome da pessoa + `/<handle>-mode` + "work in their style", não em palavras-chave genéricas como "write code" ou "review PR".
- Formatação do frontmatter. Siga as regras de YAML do `create-skill`. Mantenha `description` como um escalar YAML único. Ponha entre aspas ou use `description: >-` com linhas de continuação indentadas quando a pontuação ou a quebra exigir.
- `disable-model-invocation: true` no frontmatter por padrão. Abra exceção só se o usuário quiser explicitamente que o modo dele se aplique em todo turno.

### 5. Itere na prosa

Aplique a skill **unslop** e as diretrizes de escrita do `create-skill` em cada linha.

Mostre o rascunho ao usuário e receba feedback. Espere várias iterações. Corte sem dó. Uma skill de modo não é um manual.

### 6. Aterrisse

Trabalhe num worktree a partir da main. Faça commit e abra uma PR. Não faça push direto na main.

## Travas

- **Não sobreajuste a uma conversa.** Uma preferência dita uma vez e contradita em outra é ruído. Exija múltiplas ocorrências antes de codificar.
- **Não seja esperto.** Reafirmar o conteúdo de outras skills, inventar metáforas ou escrever prosa "poética" para um leitor agente é custo sem benefício. Mantenha operacional.
- **Referencie, não embuta.** Outras skills em que o usuário se apoia devem aparecer como referências de caminho, não como trechos colados. O mesmo para qualquer doc de princípio que ele mantenha em outro lugar.
- **Mantenha as seções mínimas.** Só adicione uma seção se o usuário tiver ali uma regra específica, fora do padrão. "Communicate clearly" não é seção. "Short paragraphs. Tables when comparing options. Bullets only when items are genuinely parallel." é.
- **Nomeie convenções de forma genérica.** Use "the user" ou "the human" nos imperativos, não o primeiro nome do autor.
- **Não force simetria.** Se o usuário não tem regras de processo que valham o registro, pule a seção Processo por inteiro.

## Avaliação

Uma skill `-mode` é saída subjetiva. Um laço de benchmark de testar/iterar no estilo do `create-skill` não serve aqui. Cheque a sensação com o usuário. Isso soa como ele? Faltou algo? Depois entregue.

Rode um laço de otimização de description só se a precisão de gatilho se mostrar um problema na prática.

## Quando não usar

- O usuário quer uma skill específica de tarefa (não convenções de trabalho). Só `create-skill`, sem mineração.
- O usuário quer capturar um fluxo estreito (ex. "how I write commit messages"). Isso é uma skill comum, não uma skill de modo.
