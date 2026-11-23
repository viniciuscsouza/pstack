---
name: principle-build-the-lever
description: "Aplique a qualquer trabalho não trivial, não só a trabalho em massa: edições, migrações, análises, checagens. Construa a ferramenta que faz ou prova aquilo (codemod, script, gerador, ou uma skill que seus subagentes seguem) em vez de trabalhar na mão. A ferramenta é o artefato que um revisor roda de novo."
---

# Construa a Alavanca

Quando o trabalho não é trivial, construa a ferramenta que o faz (um codemod, script, gerador, ou checagem rerodável) em vez de fazer na mão. O critério é a trivialidade, não a repetição. Pule a ferramenta só para duas ou três edições óbvias.

- Faça a primeira unidade na mão, depois construa a ferramenta e prove rodando ela de novo naquela unidade. Torne seguro rodar de novo.
- Uma ferramenta determinística vence o leque. Quando você fizer leque, escreva a receita, o contrato de verificação e as cercas como uma skill que os delegados leem e não podem editar.
- Aplicar isto produz um arquivo. Nenhum script, codemod, gerador ou skill de delegado no diff significa que você não aplicou. Commite quando o trabalho sobrevive à sessão.
- Construa o menor script que faz ou prova o trabalho, nunca um framework.
