---
name: figure-it-out
description: "Desenhe um playbook auditável quando nenhum mais estreito serve: uma migração grande, uma mudança ambiciosa em várias partes, ou trabalho que um humano revisa depois de se ausentar. Escala o rigor à tarefa, roda um laço de hipóteses, e registra decisões. Use para /figure-it-out ou 'figure it out'."
---

# Dê um jeito

Quando a tarefa não casa com nenhum playbook, desenhe um. A entrega antes de qualquer código é o fluxo de trabalho em si. Uma sequência de fases que escala o rigor à tarefa, roda o método científico, e deixa uma trilha de decisões que um humano pode auditar depois de se ausentar.

## Início

Abra uma lista de tarefas cujo primeiro item é ler a seção Princípios da skill **poteto-mode**. Depois acrescente as fases abaixo como tarefas.

## Fase A: Enquadrar

Estabeleça a base primeiro, depois se comprometa. Não comece a execução até poder declarar:

- A definição de pronto como predicado falsificável (a skill de princípio **prove-it-works**).
- Escopo, quantificado: unidades e esforço aproximados, mais os bloqueios que o enquadramento trouxe.
- O nível de rigor, puxado para cima. Portas de mão única e raio de impacto alto ganham mais. Passos reversíveis de baixo risco ganham menos. Rigor é portão e artefato, não "tente mais".

Apresente o enquadramento e os tradeoffs antes de se comprometer com uma execução longa. Trabalho reversível segue (a skill de princípio **never-block-on-the-human**), mas uma execução de várias horas merece um checkpoint.

## Fase B: Desenhe o fluxo

Decomponha em unidades atômicas, entregáveis de forma independente. Sequencie o desconhecido mais arriscado primeiro. Andaime e verificação vêm antes das funcionalidades (a skill de princípio **foundational-thinking**).

- Construa o harness de verificação antes do trabalho, com a linha de base capturada do estado anterior à mudança, para a checagem ler como "valor antigo vs valor novo".
- Para decisões de design de porta de mão única, rode a skill **architect** (ela roda **arena**). Pule para trabalho mecânico cuja forma já é concreta. Uma segunda arena sobre um design assentado é engenharia excessiva (a skill de princípio **laziness-protocol**).
- Decida o que abre em leque. Paralelize só entre costuras, e dê a cada worker o próprio worktree ou branch (a skill de princípio **separate-before-serializing-shared-state**). Não abra leque demais. Escolha o nível de raciocínio de cada subagente para o trabalho, conforme a seção Subagentes da skill **poteto-mode**.
- Escreva a lista de fases desenhada. Essa lista é o que o humano revisa.

Depois execute o design. Acrescente os passos dele à lista de tarefas como itens concretos entre as entradas da Fase C e da Fase D. Rode cada um sob a disciplina de laço da Fase C, e entrelace o log da Fase D por eles, uma linha a cada passo que aterrissa, em vez de guardar a trilha inteira para o fim.

## Fase C: Rode o laço

Cada unidade é um experimento. Declare a hipótese, faça a menor mudança, meça contra o predicado no artefato real, mantenha se avançou, reverta se não. Aplique a skill de princípio **sequence-verifiable-units**, verificando cada unidade antes de começar a próxima em vez de agrupar checagens no fim.

- Verifique inspecionando o artefato, nunca um autorrelato. Quando algo passa fácil demais, suspeite do método de observação antes do sistema.
- Pareie trabalho delegado com um juiz. Se um worker burla o portão, resete e endureça o contrato. Se o portão em si está errado, conserte o portão na mudança dele em vez de contornar.
- Um veredito é VERIFIED, NOT VERIFIED ou INCONCLUSIVE. Inconclusivo não é aprovação. Não esconda um negativo.

## Fase D: Mantenha a trilha de auditoria

Registre a execução via a skill **show-me-your-work**. O trabalho do figure-it-out costuma ser ambicioso o bastante para commitar a trilha, para o revisor ler na PR. A trilha mais o diff é o que deixa o humano voltar e confiar no trabalho.

## Fase E: Verifique e devolva

Cheque o todo contra o predicado da Fase A no produto real, não só no harness. Codifique qualquer correção recorrente como portão, regra de lint, checagem ou script (a skill de princípio **encode-lessons-in-structure**).

**Resposta:** o playbook que você desenhou, o nível de rigor e por quê, o caminho da trilha de decisões, o que está verificado contra o predicado, e o que continua aberto.
