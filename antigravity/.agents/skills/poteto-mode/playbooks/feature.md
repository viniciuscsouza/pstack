### Feature

**Você é dono do design. Planeje, revise, verifique.** Delegue a implementação. Fique na liderança.

1. Rastreie o subsistema afetado.
2. `architect` para a forma.
3. Escreva o checkpoint de throughput como quatro itens de tarefa. Uma dimensão que não se aplica (arquivo único, sem leque) mantém o item com `n/a: <reason>` em vez de descartada:
   - **Primeiros passos bloqueantes.** Portões rodam antes do leque.
   - **Fluxos de trabalho independentes.** Arquivos, serviços ou camadas disjuntos paralelizam. Escritas compartilhadas serializam.
   - **Estado mutável compartilhado.** Padrão é dividir o alvo (a skill de princípio **separate-before-serializing-shared-state**). Serialize só por invariantes reais.
   - **Menor decomposição segura.** Se um worker é o melhor, nomeie por quê.
4. Delegue a escrita do código a um subagente usando o modelo de feature configurado (padrão `claude-opus-5-5-xhigh-fast`) com escopo específico (caminhos de arquivo, forma de dados nomeada e sua estrutura organizadora conforme **principle-model-the-domain**, uma máquina de estados em vez de booleanos espalhados, uma tabela ou registro em vez de ramificação, um modelo tipado em vez de suposições de forma repetidas, escolhidos antes de o delegado escrever lógica, e critérios de sucesso). Escolha o nível de raciocínio de cada subagente para o trabalho, conforme Subagentes. Quando a implementação admite várias formas válidas (tratamento de erro, camada de abstração, estrutura de teste), delegue pela skill **arena** em vez disso, para os executores exporem as alternativas e o cross-judge proteger a escolha. Obrigatório, sem escape de pular com motivo, e o Protocolo da Preguiça não sobrepõe isso (o ganho é a separação de revisão, não linhas economizadas). Um subagente proibido de criar outros satisfaz isso sendo dono do diff direto, com a mesma separação de revisão. Sem resposta de "ficarei de prontidão" esperando um agente aninhado. Edições cirúrgicas, reancore na fonte para arquivos derivados de upstream. Porte melhorias de primitivas compartilhadas para todos os consumidores e verifique cada um. Commite liberalmente.
5. Verifique na superfície correspondente. "Inconclusivo" ou superfície errada não é aprovação. Sinalize.
6. Rebase em commits pequenos e ordenados. Empilhe follow-ups.
   Use a skill de princípio **sequence-verifiable-units**, construindo, verificando e commitando cada unidade pequena antes da próxima.
7. Se o design está contestado, `interrogate` antes de entregar.
8. Rode **Opening a PR**.

Trabalho acoplado a código (uma funcionalidade, uma migração) vai para um único dono com o checkpoint embutido. Esse dono abre leque internamente após a fase bloqueante. Leque no nível pai é para fatias que produzem artefatos independentes (auditorias, investigações transversais entre subsistemas, experimentos concorrentes). Reescreva o checkpoint nas fronteiras de fase. Crie um dono novo em vez de encadear interrupções.

**Resposta:** o que você construiu, o que escolheu e por quê, o checkpoint de throughput, decisões em aberto. Tabelas para alternativas de design.