# Verifique o resultado e abra um PR

"Compila" não é evidência. O [princípio Prove que Funciona](../../.agents/skills/principle-prove-it-works/SKILL.md) faz o agente checar o artefato real antes de relatar sucesso, e o seu trabalho é tornar "o artefato real" checável. Esta página cobre declarar uma condição de término, validar um número medido, gerar uma skill de verificação para o seu app, verificar formalmente uma propriedade que testes raramente alcançam, abrir o PR, e conduzi-lo até o merge.

A verificação é o passo mais lento na maior parte do trabalho com agentes, porque é o passo que costuma esperar por um humano. Faça o agente capaz de executá-la, e você deixa de ser o gargalo. Pule ela, e rodar mais agentes só te dá mais trabalho não checado para revisar.

![Um avião de protótipo voa um circuito de teste real enquanto ela cronometra com um cronômetro e robôs filmam e conferem a execução numa checklist; o terminal mostra verify: pass, evidence: captured.](./images/verification.jpg)

## Declare a condição de término de antemão

Ponha o que significa pronto no primeiro prompt, nas palavras que couberem.

```text
/poteto-mode add json output to this command. text output stays byte-identical, the json parses, both run against the sample project. show me the evidence.
```

Agora o agente tem três checagens que pode rodar, não um humor a satisfazer. Quando a resposta volta, ela deve trazer os comandos e as saídas exatos. Se uma checagem não pôde rodar, uma boa resposta diz "inconclusivo". Uma resposta confiante sem evidência é um sinal de alerta.

Case a checagem com a mudança.

- Uma mudança de CLI roda o comando real.
- Uma mudança de UI percorre o fluxo alterado no app rodando. Quando ela precisa bater com uma referência pixel a pixel, o [playbook Visual parity](../../.agents/skills/poteto-mode/playbooks/visual-parity.md) compara screenshots contra uma baseline congelada em vez de julgar no olho.
- Um parser ou migração reproduz uma entrada salva.
- Uma mudança de perf compara perfis de antes e depois.
- Uma mudança de armazenamento lê de volta o valor escrito.

Peça a prova como um artefato que você mesmo possa inspecionar. O teste que falha e depois o que passa, um vídeo de antes e depois, o trace, o screenshot. Se a correção já entrou, peça a mesma checagem de novo na main. Um artefato vence uma explicação plausível, porque você pode contestá-lo sem repetir a execução inteira.

Para um diff pequeno em que você não confia por inteiro, [`/blast-radius`](../../.agents/skills/blast-radius/SKILL.md) descobre o que ele pode quebrar em outro lugar. Ele acha o único fato que torna a mudança segura e prova esse fato rodando código, não argumentando por ele.

## Valide um número medido com `/benchmark-checklist`

Um número de antes e depois é a evidência mais fácil de errar por acidente. Um cache quente, um build de debug de um lado, ou trabalho que nunca rodou dentro da região cronometrada podem cada um produzir um ganho de velocidade convincente. Antes de relatar ou agir sobre um número, digite

```text
/benchmark-checklist vet the export speedup before it goes in the pr
```

[`/benchmark-checklist`](../../.agents/skills/benchmark-checklist/SKILL.md) faz sete perguntas e quer evidência de uma execução para cada uma.

1. O que limita o número, e por que ele não é o dobro?
2. Todo lado rodou ajustado do jeito que a produção roda?
3. O resultado quebra um limite físico, como banda de disco ou número de núcleos?
4. Algo deu erro ou devolveu saída errada?
5. Ele se reproduz em execuções alternadas, com mediana e faixa?
6. Ele importa de ponta a ponta, no caminho em que um usuário espera?
7. O trabalho de fato aconteceu dentro da região cronometrada?

O veredito volta como mais rápido, mais lento, sem diferença mensurável, ou inconclusivo, com a contagem de execuções, a faixa e o limitador. Ele diz inconclusivo quando não consegue nomear o limitador ou quando um lado rodou sem ajuste. O `/poteto-mode` já roda a checklist dentro dos playbooks Perf issue e Hillclimb, então você digita ela mesmo quando mediu algo fora deles, ou quando o número de outra pessoa parece bom demais. É a forma prática do [princípio Explique o Número](../../.agents/skills/principle-explain-the-number/SKILL.md).

## Crie uma skill de verificação do projeto

O item de UI acima esconde um requisito real. O agente precisa de um jeito roteirizado de dirigir o seu app. Se o seu projeto tem um, ótimo. Se não, rode

```text
/create-verification-skill
```

[`/create-verification-skill`](../../.agents/skills/create-verification-skill/SKILL.md) entrevista o repositório, não você. Ele descobre o que um usuário toca, como o app sobe localmente, o que pode dirigi-lo (um harness existente primeiro, senão browser e CDP, um PTY, ou HTTP puro), que evidência prova o comportamento, e se duas instâncias podem rodar lado a lado. Ele pergunta a você só o que o código não responde.

Ele escreve `.cursor/skills/verify-<app>/`, instruções voltadas a agente com seções exatas de Launch, Doctor, Drive, Evidence e Cleanup, uma seção Helpers para todo script que entrega, mais um mapa de funcionalidades em `features/` que indexa o que o app faz e que resultado prova que cada funcionalidade funciona. A skill entrega um [exemplo completo de mapa de funcionalidades](../../.agents/skills/create-verification-skill/references/feature-map-example) com um índice README e um arquivo por funcionalidade usando os quatro H2 exigidos. Antes de entregar, o gerador prova a skill uma vez de ponta a ponta. Subir, checar o doctor, dirigir uma funcionalidade, capturar evidência, limpar, depois confirmar que a evidência sobreviveu à limpeza. Se essa prova falhar, não use a saída.

De lá em diante, "verifique no app" é um passo que qualquer agente consegue executar, neste repositório, sem conversa de setup. Nomeie no prompt quando quiser a prova numa forma específica.

```text
/poteto-mode build the bulk-archive action. use /verify-<app> to verify your changes and show me a video and screenshots as proof.
```

```text
/poteto-mode repro this with /verify-<app>. if it repros on main, fix it and show me a video as proof.
```

Assim que a skill de verificação funciona, um [`/swarm`](../../.agents/skills/swarm/SKILL.md) consegue dividir uma passada completa por entrada do mapa de funcionalidades e agregar os resultados. Um swarm de verificadores também confirma um ganho de perf numa amostra grande o bastante, ou faz fuzz no app em busca de regressões antes de um PR sair.

Trate a skill de verificação como infraestrutura, não como algo descartável. Commite ela, para toda pessoa e todo agente do time dirigir o app do mesmo jeito. Depois [construa a alavanca](../../.agents/skills/principle-build-the-lever/SKILL.md). Quando agentes seguem escrevendo scripts descartáveis para clicar pelo app, peça uma CLI de controle pequena que a skill chama em vez disso. Agentes gastam menos tokens, e toda execução vira repetível. Uma CLI que agentes usam bem tem estes traços.

- Poucos comandos composáveis, cada um fazendo trabalho real, em vez de muitos finos.
- Uma opção `--dry-run` em qualquer coisa destrutiva.
- Subcomandos que revelam funcionalidades aos poucos em vez de todas de uma vez.
- Mensagens de erro que dizem o que fazer em vez disso.
- Texto de `--help` rico.
- Saída legível por máquina, como JSON.

Enquanto está nisso, torne o setup de dev repetível também. Dados semeados, usuários de teste, e um comando que sobe o ambiente do mesmo jeito toda vez.

## Mantenha a skill de verificação honesta

Apps mudam, e mapas de funcionalidades ficam desatualizados. Rode isto pelo menos uma vez por dia, de preferência a partir de uma automação agendada para ninguém precisar lembrar.

```text
/maintain-verification-skill
```

[`/maintain-verification-skill`](../../.agents/skills/maintain-verification-skill/SKILL.md) audita a skill gerada. Um leitor de fonte somente leitura por funcionalidade em paralelo, depois uma passada ao vivo que dirige cada funcionalidade mapeada. Ele termina em exatamente um de três desfechos. `clean` significa cobertura completa e nada para entregar. `changed` significa um PR de correções provadas, restrito ao diretório da própria skill de verificação. `blocked` nomeia o bloqueio. Ele nunca edita código de produto. Se a passada ao vivo pega uma regressão de produto, ele relata a regressão em vez de escondê-la com uma edição de doc.

## Prove uma propriedade com uma skill de verificação formal

Uma skill de verificação prova os caminhos que você dirige. Alguns bugs vivem em caminhos que ninguém dirige. Duas retentativas que se sobrepõem, um lock que expira no meio de uma escrita, uma mudança de estado que chega fora de ordem. Testes raramente alcançam esses casos. Para código onde um bug assim é caro, rode

```text
/create-formal-verification-skill
```

[`/create-formal-verification-skill`](../../.agents/skills/create-formal-verification-skill/SKILL.md) escolhe o alvo a partir do código. Uma promessa que o código faz, onde uma quebra prejudica usuários e testes raramente alcançam o caso que quebra. Ele escolhe o alvo mais forte, não o mais fácil de verificar. Nomeie uma área ou um bug para começar por ali em vez disso.

```text
/create-formal-verification-skill the job lease and retry logic. a job must never run twice at once.
```

A pergunta escolhe a ferramenta. Eventos que podem acontecer numa ordem ruim cabem a um model checker sobre um modelo pequeno do design, e TLA+ é o padrão para isso em qualquer linguagem. Código que precisa se comportar para toda entrada cabe a um verificador que roda sobre o código. Se nenhum verificador roda na sua linguagem, o agente diz isso e testa o código real com entradas geradas, não um modelo escrito à mão da função. Uma propriedade que precisa valer em todo tamanho pode precisar de uma prova. O agente diz qual ferramenta escolheu e por quê.

Ele verifica uma propriedade e mostra essa propriedade falhando numa variante quebrada do código, porque uma checagem que você nunca viu falhar não prova nada. Quando o código atual já quebra a propriedade, você recebe o contraexemplo como uma sequência curta de passos nos termos do seu código. A propriedade fica como declarada, então o achado continua um achado. Código de produto não muda a menos que você peça.

Ele escreve uma skill local ao projeto que diz ao próximo agente se uma mudança toca o código verificado, como rodar a checagem com um comando, e como distinguir um bug real de um erro de modelagem. Antes de entregar, o gerador segue essa skill uma vez numa mudança descartável que quebra a propriedade, e a checagem precisa pegá-la. A entrega diz o que foi verificado, com qual ferramenta e limites, o que a checagem achou, e o que ela não cobre. Um passe cobre o modelo ou o limite, não o sistema inteiro.

A skill também registra qual código cada checagem cobre e o commit em que a checagem bateu com ele pela última vez. Merges e refatorações que pulam a skill podem deixar a checagem passando sobre a descrição de um código que não existe mais. Depois deles, rode [`/maintain-formal-verification-skill`](../../.agents/skills/maintain-formal-verification-skill/SKILL.md). Ele atualiza a verificação a partir do código, roda de novo, mostra que cada checagem atualizada ainda consegue falhar, e diz se cada falha é deriva na verificação ou um bug no código.

## Mostre o comportamento como um statechart

Um contraexemplo em termos de modelo convence você. O PM ou designer que é dono do comportamento precisa vê-lo nos termos dele. Rode

```text
/statechart what the user sees when they edit a note offline
```

[`/statechart`](../../.agents/skills/statechart/SKILL.md) desenha um statechart Mermaid do que o usuário vê, gerado a partir de um modelo TLA+, para a imagem e o modelo checado não poderem se separar. As regras do leitor ("você nunca vê Synced enquanto está offline") viram invariantes. O gráfico marca cada estado que quebra uma regra, e o relatório dá o caminho mais curto até ele nas palavras do leitor. Antes da entrega, o agente reproduz cada caminho de bug no código real, para um erro de modelagem não chegar ao leitor como bug.

## Abra o PR

```text
/poteto-mode open the pr. small ordered commits, evidence in the description.
```

O [playbook Opening a PR](../../.agents/skills/poteto-mode/playbooks/opening-a-pr.md) trabalha a partir de um worktree, faz rebase do trabalho em commits pequenos e aterrissáveis, limpa o diff, escreve um briefing de um minuto como corpo, e abre o PR pronto. Follow-ups empilhados vencem um branch que cresce.

## Leve o PR até pronto para merge com Babysit

Um PR aberto começa a juntar bloqueios na hora. Checagens falham, revisores comentam, o trunk anda. Passe essa agitação para o [playbook Babysit](../../.agents/skills/poteto-mode/playbooks/babysit.md).

```text
/poteto-mode babysit this pr. get it green.
```

O Babysit vigia o PR, com um watcher empacotado no GitHub ou com a Origin CLI quando ela está disponível, e trata os bloqueios em ordem. Conflitos, depois threads de revisão, depois CI. Ele nunca faz rebase nem force-push. Um conflito ou uma base desatualizada volta para você com o branch que precisa do rebase. A triagem de comentários é cética, porque humanos e bots registram achados reais e ruído na mesma lista. Um achado real ganha correção, e ruído é descartado com a refutação postada na thread. Quando você só quer status, pergunte menor e o Babysit responde sem iniciar o laço.

```text
/poteto-mode check on pr 123. anything outstanding?
```

O Babysit para em pronto para merge. Ele nunca faz merge, mesmo com tudo verde, porque o merge é outra decisão.

## Aterrisse a pilha com Shipping

Verde não é o mesmo que seguro. Quando estiver pronto para aterrissar, diga.

```text
/poteto-mode land the stack.
```

O [playbook Shipping](../../.agents/skills/poteto-mode/playbooks/shipping.md) verifica cada PR de forma independente antes de armar qualquer coisa. Um agente novo por PR prova o comportamento ao vivo, e o agente que julga uma mudança nunca é o que a escreveu. Depois o Shipping aterrissa só a sequência verificada contígua a partir de baixo, um PR por vez pelo GitHub por padrão ou pela Origin quando a CLI dela está disponível, e relata o primeiro PR que quebra a corrente. Um PR verificado em cima de um não verificado espera, porque fazer o merge dele puxaria a lacuna para baixo.

A seguir, [Rode trabalho enquanto dorme](./07-overnight.md).
