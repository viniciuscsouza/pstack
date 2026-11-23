---
name: no-comments
description: "Crie o Comment Sicko, corrija os achados aceitos, e ofereça codificações para restrições alegadas."
disable-model-invocation: true
---

# Sem comentários

Crie o Comment Sicko. Aja sobre os achados aceitos.

Respeite a perspectiva nova do Comment Sicko.

## Escopo

Use os arquivos ou o diff do chamador. Caso contrário, use o diff atual contra o branch base, `main` por padrão, incluindo a árvore de trabalho.

## Passos

1. Crie um `Task` com `subagent_type: "comment-sicko"`. Passe o escopo. Escolha o nível de raciocínio de cada subagente para o trabalho, conforme a seção Subagentes da skill **poteto-mode**. Não repita as regras dela.
2. Inspecione o relatório e o diff dele. Rejeite edições em código de aplicação, fugas de escopo, deleções protegidas por exceção, motivos de `MUST KILL` mal descritos, e flags que tratam código intencional mantido como culpado. Flags sobre surpresas em código nosso seguem acionáveis. Não restaure esses comentários. Uma preservação só sobrevive com prova de que trata de algo imutável. Audite supressões de lint e TypeScript com escopo que passaram despercebidas. Supressões de correção ou de segurança seguem `MUST KILL` acionáveis. Restaure deleções só com exceções exatas e prova com escopo. Antes de aceitar derrubadas ou preservações magras de `IMPORTANT` ou `do not remove`, rastreie o símbolo ou rode `/why` nele. Se uma derrubada está ambígua, não restaure. Se uma preservação é refutada ou ainda ambígua, apague. Reverta e rode de novo um relatório rejeitado, com a falha nomeada. Rejeite um segundo, reporte aberto, e falhe o `/no-comments`.
3. Corrija flags triviais aceitas direto, apagando um caminho morto, tirando um parâmetro, ou usando a API real. Se alguma correção precisa de forma, rode `/architect` uma vez para o conjunto aceito e o código ao redor. Pare no esboço. O architect dá forma. O passo 4 implementa.
4. Implemente a menor correção de causa raiz dentro do escopo. Remova todo contorno nomeado. Se a causa raiz está fora do escopo, entregue a menor correção dentro do escopo e reporte o resto aberto. As skills **principle-fix-root-causes** e **principle-redesign-from-first-principles** guiam só a intenção. Nenhuma das duas autoriza alargar a cerca nem corrigir instâncias fora dela. Nunca parafuse guardas de sintoma.
5. Comentários de restrição dizem `do not remove`, `do not change wording`, ou `talk to X before changing`. Mantenha preservações sobre coisas imutáveis. Ofereça o tipo, runtime, teste ou lint de CI mais barato dentro do escopo. Espere aprovação interativa. Desassistido e eval exigem pré-aprovação do chamador. Se aprovado, codifique e apague. Senão, apague, reporte a restrição aberta, e esboce o trabalho fora do escopo.
6. Reporte a contagem de deleções, comentários restaurados, reexecuções, esboço do architect, correções, ofertas de codificação, codificações, restrições não impostas, e outro trabalho aberto.
