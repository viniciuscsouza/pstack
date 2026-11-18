### Session pickup

**O ponto de retomada é seu. Leia o rastro anterior, não refaça ele.**

1. Localize o rastro anterior. Um transcript local sob o diretório `agent-transcripts/` do workspace ativo (o system prompt nomeia o caminho. Não faça glob por `~/.cursor/projects/*/`, isso cruza fronteiras de workspace e lê chats privados de projetos alheios), uma URL de agente na nuvem, ou um branch já enviado. Leia a visão geral de metadados e as últimas mensagens primeiro, depois varra para trás procurando os pontos de decisão. Faça parse de um transcript longo num subagente e mantenha a linha do tempo reduzida na thread principal (a skill **principle-guard-the-context-window**). Escolha o nível de raciocínio de cada subagente conforme a seção Subagentes.
2. Reconstrua o estado operacional. O branch e o worktree, o que já aterrissou (`git log`, `git diff` contra a base), os todos abertos, as decisões tomadas. O rastro anterior é entrada autoritativa. Resista à tendência de rederivar ele.
3. Faça o diff entre feito e pendente. Compare o que foi entregue com o que foi planejado, nomeie o ponto de retomada, não rode de novo o repro anterior nem refaça trabalho concluído. Uma passada de "deixa eu verificar do zero" significa que você trata o rastro como não confiável quando ele é autoritativo.
4. Roteie o trabalho restante para o playbook correspondente e escolha o veredito. Continue a execução, entregue uma recomendação pronta, ratifique ou derrube uma conclusão anterior, ou faça postmortem de uma execução que falhou. O playbook de retomada termina aqui. O playbook roteado é dono do resto.
5. Verifique as afirmações herdadas contra o objetivo original no artefato real (a skill **principle-prove-it-works**). Um autorrelato anterior que passou não é a prova.

**Resposta:** onde o agente anterior parou, o que você herdou versus refez (idealmente nada refeito), o ponto de retomada, e o desfecho.
