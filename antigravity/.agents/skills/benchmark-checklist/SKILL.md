---
name: benchmark-checklist
description: "Avalie uma medição de perf (limitador, ajuste, limites, erros, reprodutibilidade, relevância, e se o trabalho aconteceu) antes de relatar ou agir sobre ela. Use quando você roda um benchmark ou relata um ganho de velocidade ou regressão que mediu."
---

# Checklist de benchmark

Use isto quando você produz um número de performance: o antes e depois de um PR, uma alegação de regressão, um harness de hillclimb, ou uma escolha de biblioteca ou config. Responda cada pergunta com evidência de uma execução, não com um palpite sobre o código.

Para uma estimativa rápida que o usuário pediu, uma execução basta. Ainda assim cheque as perguntas 4 e 7, e diga que é uma execução. Pule o resto a menos que essa execução pareça errada. Uma escolha entre opções nunca é estimativa.

## Antes de rodar qualquer coisa

- Escreva a afirmação que você espera fazer, nas palavras que você entregaria ("a exportação é 30% mais rápida em p50 no dataset de 60k linhas"). As perguntas testam essa frase.
- Leia o script de medição. Anote o que ele cronometra, conta e ignora.
- Cheque o load average com `uptime` e a contagem de núcleos com `nproc`. Se a máquina está ocupada, descubra o que roda. Se não conseguir parar, intercale os lados para ambos verem o mesmo ruído, e diga isso no relatório.

## As perguntas

1. **Por que não o dobro?** Nomeie o limitador. Faça profile numa execução que você não relata, porque profilers e tracers deixam o trabalho mais lento. Use CPU por processo (`top`, `pidstat`), um profiler de runtime (`node --cpu-prof`, `py-spy`, `perf`), espera de I/O, e contagem de syscalls (`strace -c` no Linux), depois mapeie o ponto quente para o código-fonte. Vigie o gerador de carga também. Se ele satura primeiro, você mediu o gerador de carga. Se uma mudança não moveu o número, o limitador explica por quê, então ache ele antes de chamar a mudança de inútil.
2. **Estava ajustado?** Rode cada lado do jeito que a produção roda: builds de release, flags e env de produção, ajustes de batching e transação, pools de conexão, caches tão quentes ou frios quanto a produção vê, e as mesmas versões e dados. Se um lado roda com padrões, você comparou configurações, não implementações. Um limitador que é uma configuração (um commit por linha, um build de debug, um índice faltando) significa que aquele lado está sem ajuste. Ajuste e meça de novo antes de escolher um vencedor. Se não conseguir ajustar, não escolha vencedor a partir dessa execução. Quando o usuário está escolhendo o que adotar, estreitar a afirmação para o código como ele entrega hoje não resolve, porque ele adota a opção, não as configurações de hoje.
3. **Quebrou limites?** Faça a aritmética. Compare bytes por segundo com a banda de disco e de rede, operações por segundo vezes custo por operação com seus núcleos, e o tempo economizado com o tempo que a peça mudada levava. Remover uma peça que leva 10% da execução deixa a execução no máximo cerca de 11% mais rápida. Um resultado acima de um limite mediu algo diferente do trabalho, como um cache, um no-op, ou um bug.
4. **Deu erro?** Conte falhas e respostas não-sucedidas, e cheque que as saídas estão corretas, não apenas presentes. Rejeições costumam ser rápidas, e timeouts e retentativas são lentos. Se o script não conta erros, adicione a contagem.
5. **Reproduz?** Rode cada lado pelo menos 5 vezes, alternando (A, B, A, B, e assim por diante) para warmup, inicialização preguiçosa, caches e deriva não favorecerem nenhum. Relate a mediana e a faixa. Uma diferença menor que a variação entre execuções não é diferença mensurável. Quando a decisão está apertada, use um teste de soma de postos ou a estatística do próprio harness.
6. **Importa?** Ao lado de qualquer micro result, meça o caminho de ponta a ponta que um usuário espera, com tamanhos de dados e concorrência realistas, e relate o micro result como fração do todo. Um helper que leva 1% de um request deixa o request no máximo 1% mais rápido, por mais rápido que o helper fique.
7. **Aconteceu mesmo?** Confirme que o trabalho rodou dentro da região cronometrada. O request chegou ao servidor, as linhas foram escritas, os bytes foram lidos, e o código usou o resultado. Código preguiçoso (generators que ninguém itera, promises que ninguém aguarda, resultados que o JIT pode descartar) e timeouts produzem números para trabalho que nunca aconteceu.

## Relatório

- Comece pelo veredito: mais rápido, mais lento, sem diferença mensurável, ou inconclusivo.
- Dê o número com a unidade, a contagem de execuções, a faixa e o limitador. Por exemplo, "p50 41 ms → 33 ms, mediana de 7 execuções por lado, faixa 32 a 35 ms depois, limitado pelo parse de JSON num núcleo."
- Chame o veredito de inconclusivo quando você afirma uma diferença mas não consegue nomear o limitador, quando um lado rodou sem ajuste, ou quando você não conseguiu checar as perguntas 4 e 7. Nomeie a lacuna.
- Mantenha o corpo de um PR com um número principal. Ponha as execuções, a faixa e a evidência do limitador num artefato linkado ou arquivo de notas.

## Outro material de perf

- Avalie a linha de base do playbook **Perf issue** antes de o playbook planejar a partir dela, e todo número depois disso.
- Avalie o harness do playbook **Hillclimb** antes de ele ser congelado. O harness congelado passa a imprimir contagens de erro e de trabalho, então cada manter-ou-reverter checa as perguntas 4 e 7 de graça.
