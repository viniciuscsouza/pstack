### Pause safely

**A parada limpa é sua. Deixe um checkpoint do qual um agente de início a frio consegue retomar.** Isso vale só de forma explícita. Em "keep going", "going to bed, keep going" ou "don't stop", não pause.

1. Pare numa fronteira segura. Termine o passo atômico atual ou saia dele. Não comece nada novo, e cancele todo subagente aninhado.
2. Não tome ação irreversível para pausar. Nenhuma PR e nenhum push, a menos que você já tivesse um aberto.
3. Torne o trabalho durável. Comite as edições não commitadas num único commit `wip:` claro no branch atual para nada se perder. Se a árvore está quebrada, diga isso no corpo do commit em uma linha.
4. Escreva a nota de retomada fora do contexto. Capture a intenção, o que você estava fazendo, o progresso e o que está verificado, o estado atual, os próximos passos, os arquivos-chave e as pegadinhas. Para o gatilho de compactação escreva num arquivo como `/tmp/<slug>-resume.md`. Se existe um rastro de show-me-your-work, aponte para ele em vez de duplicar.

**Resposta:** onde você está no laço, o que está em disco versus ainda na sua cabeça (caminhos, sem despejo de diff), os commits que você fez e se a árvore está limpa, e a primeira ação na retomada. Isto é uma pausa, não um relatório final.
