---
name: principle-sequence-verifiable-units
description: "Aplique a trabalho de várias etapas (varreduras, migrações, sequências de edições parecidas) e a como você empilha commits e PRs. Quebre o trabalho em unidades pequenas que terminam cada uma num estado verificável, cheque cada uma antes da próxima, e ordene a entrega para a sequência se provar a um revisor."
---

# Sequencie o trabalho em unidades verificáveis

Ordene o trabalho em unidades pequenas que terminam cada uma num estado que você consegue checar, e não avance até a atual estar verde.

Numa varredura, migração ou sequência de edições parecidas, cada unidade é um bracket. Estado sabidamente bom, uma mudança, rode a checagem, prossiga. Rode a checagem por unidade mesmo quando uma ferramenta faz as edições.

Entregue na ordem que prova o trabalho. O teste que falha primeiro, depois a correção. A subtração antes da reforma. A linha de base antes do tratamento. Cada commit aterrissa sozinho.
