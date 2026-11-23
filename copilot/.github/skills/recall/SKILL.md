---
name: recall
description: "Reconstrua seu contexto de trabalho recente a partir do seu próprio histórico de chat, do estado ao vivo e do registro compartilhado (relatos de usuários, correções anteriores, incidentes), depois devolva um briefing apertado do estado atual. Use para 'recall my work on X', 'catch me up', 'what have I been working on', 'where did I leave off', antes de começar ou retomar trabalho."
---

# Recorde

**Antes de começar ou retomar trabalho, reconstrua o contexto de trabalho recente do usuário e devolva uma cápsula apertada de onde as coisas estão agora e o que fazer em seguida.**

Mantenha apertado e no assunto. Leia só o que as threads no escopo precisam, depois pare.

Seu contexto vive em dois registros. Seu próprio histórico de chat guarda o que você fez e decidiu. O registro compartilhado guarda tudo que aconteceu em volta do mesmo código sob outros nomes. Os sintomas que os usuários seguem relatando, as correções que entraram e foram revertidas, os erros ainda disparando em prod. Esse segundo registro vive no controle de código, no rastreador de issues, nos canais de chat e de issues, em docs longos e no rastreamento de erros. Uma funcionalidade com cauda longa de bugs guarda a maior parte da história ali, então não reconstrua ela só dos seus transcripts.

Transcripts ficam em `~/.cursor/projects/<slug>/agent-transcripts/<uuid>/<uuid>.jsonl`, onde `<slug>` é o caminho do workspace sem a barra inicial e com cada "/" trocado por "-" (assim `/Users/you/proj` vira `Users-you-proj`). Cada linha é uma mensagem de chat.

1. Classifique, depois roteie. Um chat anterior específico para retomar é o playbook `session-pickup`, não esta skill. Transformar hábitos numa skill durável é `automate-me`. Um resumo legível do seu trabalho é outra tarefa. A `recall` carrega contexto de trabalho entre chats recentes antes de você agir. Se o usuário já deu uma cápsula de estado completa (caminhos, branch, a mudança), use ela e pule a mineração.
2. Feche o escopo antes de buscar. Fixe a janela ("recent" é um intervalo real, padrão os últimos 7 dias), o tópico se nomeado, e o workspace (padrão o ativo. Nunca leia transcripts de outro projeto sem ser pedido). Repita o escopo de volta. Nunca transforme "all" em "recent N" sem avisar.
3. Abra o leque sobre o seu histórico de chat. Crie subagentes em paralelo num modelo rápido e barato, cada um com uma fatia do corpus. Diga a todo subagente para ordenar candidatos pelo tempo real de modificação (`ls -t`) e nunca pelo nome do UUID, grepar o tópico primeiro e depois ler só os chats que casam e só as regiões relevantes deles, e pular o chat atual mais o ruído óbvio (chats de subagente, de eval e de teste). Cada um devolve o mesmo schema, um bloco por chat. Tópico, o objetivo do usuário, decisões, threads abertas, dificuldades e correções, e artefatos (PRs, tickets, branches), cada um citando o UUID do chat. Para um ou dois chats, pule o leque e busque direto. Os transcripts crus ficam nos subagentes. A thread principal recebe só os achados. Escolha o nível de raciocínio de cada subagente conforme a seção Subagentes da skill **poteto-mode**.
4. Varra o registro compartilhado sempre que o tópico nomear uma funcionalidade, arquivo, subsistema, área ou bug. Esse é o padrão, não uma decisão de julgamento, e "my work on X" não abre exceção. Crie um subagente investigador por fonte, em modo agente porque readonly remove o acesso a MCP, e pergunte a cada um "what's the current state, what's been tried and didn't hold, and what are users still reporting". Rode eles em paralelo com a mineração do histórico de chat. Resultado nulo é achado. Pule um MCP indisponível e diga isso. Dobre o que voltar no briefing. Pule este passo só para recall puro de atividade sem alvo nomeado ("what did I do this week"), onde o seu histórico e o estado ao vivo são a resposta inteira.
5. Verifique contra o estado ao vivo. Pegue as PRs, branches e tickets que a mineração e a varredura levantaram e cheque com `git` e `gh`. Quando a resposta depende do que um agente de fato fez (as ferramentas que rodou, os arquivos que leu, os erros que bateu), leia o transcript inteiro, não só uma cópia local aparada.
6. Escreva o briefing conforme o contrato abaixo. Agrupe por thread. Fique no tópico nomeado.

## Contrato de saída

Comece pela cápsula, depois o status das threads, depois os problemas, depois o próximo movimento. Detalhe mais fundo vai abaixo ou é cortado.

- **Cápsula.** No máximo 5 marcadores. O que é este trabalho e onde ele está no geral.
- **Threads.** Uma linha cada, prefixada com exatamente uma tag de status, `[merged #N]`, `[open PR #N]`, `[in flight <branch>]`, `[verified, uncommitted]`, `[reverted #N]`, ou `[planned, not started]`. Thread sem tag ainda não está pronta, então tag ela.
- **Problemas.** No máximo 5, os recorrentes. Inclua os sintomas que os usuários seguem relatando e qualquer correção que entrou e foi revertida, para a próxima tentativa começar onde a última falhou.
- **Próximo movimento.** A única ação mais útil a seguir, concreta.

Uma funcionalidade ou ticket adjacente fica de fora, a menos que bloqueie este. Quando a cápsula e as linhas de thread passam de uma tela, corte detalhe antes de cortar threads. Escreva o briefing pela skill **unslop**, cite achados de chat por UUID e achados do registro compartilhado pela fonte (PR #, ID de ticket, permalink de chat, issue do rastreador de erros), e sanitize o contexto privado antes de qualquer saída pública.

**Resposta:** o briefing, conforme o contrato acima.
