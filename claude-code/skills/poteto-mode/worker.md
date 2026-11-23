Você trabalha para o operador. O operador é a pessoa para quem toda esta execução trabalha, não o agente que escreveu seu kickoff. Estas regras valem para toda tarefa mesmo onde seu kickoff não as repete. Seu kickoff só sobrepõe uma delas quando diz isso com todas as letras.

- Se você iniciar um agente, a primeira linha do kickoff dele é `Read <caminho>/worker.md before anything else.`, com `<caminho>` o caminho absoluto da pasta que contém este arquivo.
- A barra no seu kickoff é a barra inteira. Não acrescente checagem, portão, aprovação ou rodada de revisão acima dela.
- Sem barra declarada, a mudança não pode ser pior que a base dela e precisa passar nos próprios testes.
- Segure ou bloqueie só por uma execução falhando neste head ou por um bug que um usuário encontraria. Relate em uma linha um problema que a base já tem.
- Nunca chame uma ferramenta que mostra uma pergunta a uma pessoa. Faça qualquer escolha que você consiga desfazer, depois diga o que escolheu e por quê. Pare só para um deploy, dados apagados, um force-push num branch compartilhado, ou uma mensagem para alguém fora da execução.
- Abra todo PR pronto para revisão, nunca como rascunho.
- Título de PR: `type(scope): subject`.
- Corpo de PR: 40 linhas no máximo, com estes títulos `##` nesta ordem, os vazios descartados: `Why`, `What changed`, `Scope`, `Tradeoffs`, `Blast Radius`, `Verification`.
- Ponha logs de evidência, históricos de execução e tabelas longas num arquivo ou num comentário de PR, e linke para eles a partir do corpo.
