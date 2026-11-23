# Faça do seu jeito

O poteto-mode é o estilo de uma pessoa. Os playbooks, o roteamento e os papéis de modelo por baixo dele funcionam igualmente bem com o seu. Esta página cobre gerar um modo pessoal, capturar lições de uma sessão, consertar o repositório para agentes pararem de repetir erros, escrever uma skill focada, e testar uma mudança de skill antes de confiar nela.

Comece menor do que você pensa. Você não precisa de muitas skills no primeiro dia, nem deste plugin inteiro. Faça prompts simples, observe onde os agentes falham, e adicione uma skill ou uma checagem quando a mesma falha aparece duas vezes.

## Gere o seu próprio modo com `/automate-me`

```text
/automate-me
```

Você não descreve o seu estilo, porque o [`/automate-me`](../../.github/skills/automate-me/SKILL.md) lê ele do seu histórico. Ele minera seus transcripts recentes no workspace ativo em busca de preferências repetidas, em como você gosta de respostas, delegação, verificação, código, prosa e processo, depois pergunta quais padrões são de fato você. Ele rascunha `.cursor/skills/<seu-nome>-mode/SKILL.md` pelo fluxo `create-skill` nativo do Cursor, passa o rascunho pelo [`/unslop`](../../.github/skills/unslop/SKILL.md), e abre um PR a partir de um worktree para você revisar como qualquer outra mudança.

Rode de novo sempre que seus hábitos mudarem.

```text
/automate-me update my mode skill with everything since its last edit
```

O modo de atualização minera só o histórico desde a última mudança da skill. Ele mantém regras que você não contradisse, revisa as que têm evidência nova, e adiciona seções só para padrões novos.

## Capture as lições de uma sessão com `/reflect`

Logo depois de uma tarefa que te ensinou algo, rode

```text
/reflect that took way too long. capture what we learned so the next run doesn't repeat it.
```

[`/reflect`](../../.github/skills/reflect/SKILL.md) lê o transcript, mantém só lições que mudariam o trabalho de um agente futuro, e separa as propostas em `Accepted`, `Rejected` e `Backlog`. Ele espera a sua aprovação antes de qualquer mudança em skill. Aprove uma proposta só se ela mudaria uma decisão futura. Uma sessão estranha é anedota, não regra.

## Conserte o ambiente com `/correct`

Quando você corrige agentes pelo mesmo erro de novo e de novo, a correção pertence ao repositório, não ao seu próximo prompt. Ordene as opções por quão bem elas se sustentam.

1. Torne o erro impossível com arquitetura ou uma estrutura de dados melhor.
2. Bloqueie com tipos, ou com um lint ou checagem de CI cujo erro nomeia a correção.
3. Pegue com um teste.
4. Escreva como doc ou regra de agente. Nada falha quando um agente pula uma regra, então isso vem por último.

Revisão humana não está na lista. Um revisor que precisa pegar o mesmo erro em todo PR é o problema que isso resolve. O [`/correct`](../../.github/skills/correct/SKILL.md) faz o trabalho.

```text
/correct agents keep calling the database client directly instead of going through the repository layer
```

Ele lê commits recentes, reverts, comentários de revisão, e comentários que explicam contornos, depois agrupa os erros em classes. Uma classe conta quando já aconteceu duas vezes. Ele corrige as classes mais frequentes com um commit cada, no nível mais alto que funciona, e prova que cada checagem nova falha num erro real do passado. Ele também mantém uma tabela no arquivo de instruções de agente que pareia cada regra com o que a impõe, para uma regra que nada impõe aparecer como repetição. A resposta lista cada classe com a sua evidência, o nível escolhido, e por que um nível mais alto não funcionou.

Rode sem argumento e ele acha as classes a partir do histórico sozinho. O `/reflect` e o `/correct` dividem o trabalho. O `/reflect` melhora skills a partir de uma sessão. O `/correct` muda o repositório para uma classe de erro não poder voltar. Pareie com o `/architect` quando a correção é uma fronteira nova. [Rode muitos projetos em paralelo](./07-overnight.md#run-many-projects-in-parallel) traz um prompt que faz os dois num repositório inteiro.

## Escreva uma skill focada

Quando você já sabe o fluxo que quer capturar.

```text
/poteto-mode write a skill for verifying database migrations in this repo
```

Escrever uma skill casa com o [playbook Authoring or modifying a skill](../../.github/skills/poteto-mode/playbooks/authoring-a-skill.md), que passa pelo `create-skill` nativo do Cursor, valida o frontmatter e os links, e entrega o resultado pelo playbook Opening a PR. Prosa voltada a agente tem uma régua mais alta que prosa humana, porque uma frase inútil vira uma instrução que algum agente futuro segue. Deixe o playbook segurar essa régua em vez de escrever um `SKILL.md` à mão livre.

Dois casos especiais têm geradores próprios. Uma skill que precisa dirigir o seu app e provar comportamento é uma skill de verificação, então use [`/create-verification-skill`](../../.github/skills/create-verification-skill/SKILL.md) e [`/maintain-verification-skill`](../../.github/skills/maintain-verification-skill/SKILL.md) em vez disso. Uma skill que checa uma propriedade do seu código com um model checker, uma prova, ou outro verificador é uma skill de verificação formal, então use [`/create-formal-verification-skill`](../../.github/skills/create-formal-verification-skill/SKILL.md) e [`/maintain-formal-verification-skill`](../../.github/skills/maintain-formal-verification-skill/SKILL.md). [Verifique e entregue](./06-verify-and-ship.md#prove-a-property-with-a-formal-verification-skill) cobre os dois tipos.

## Escreva docs com um padrão usando `/technical-writing`

Skills não são a única prosa que você entrega. Para docs, RFCs, readmes, descrições de PR e mensagens de commit.

```text
/technical-writing review the readme changes
```

[`/technical-writing`](../../.github/skills/technical-writing/SKILL.md) aplica um padrão em camadas. O objetivo é prosa que um engenheiro cansado entende na primeira leitura. Ele escolhe primeiro o modo do documento (tutorial, how-to, referência ou explicação), depois trabalha frase por frase. Quem faz o quê, um pensamento por frase, nada legível de duas maneiras. Use para revisar o que você ou um agente acabou de escrever, ou nomeie de antemão quando pedir um doc.

## Teste uma mudança de skill às cegas

Uma edição de skill afeta toda sessão futura, então teste como o experimento que ela é. O mesmo vale para adotar a skill de outra pessoa. Cheque se ela melhora o agente no seu trabalho antes de mantê-la.

```text
/poteto-mode run the eval playbook on this skill change. same task for both variants, candidates stay blind.
```

Quando uma skill segue errando e você sabe o que ela deveria fazer, mude e teste numa única tarefa.

```text
/poteto-mode update the review skill so it flags missing migrations, and eval the change.
```

Pedir o eval de antemão mantém a edição honesta. Uma correção escrita a partir de uma sessão ruim tende a sobreajustar a essa sessão, e ao longo de muitas edições a skill deriva. O eval pega a deriva antes da entrega.

O [playbook Eval](../../.github/skills/poteto-mode/playbooks/eval.md) é construído em torno de um modo de falha, o efeito observador. Um agente que sabe que está sendo avaliado se comporta diferente. Então agentes candidatos recebem uma tarefa de aparência orgânica em diretórios higienizados, nunca as palavras "eval" ou "candidate", e nunca a existência um do outro. Um juiz pontua todas as saídas sob rótulos neutros, e o seguimento de cadeia é avaliado pelos arquivos que cada candidato de fato leu, não pelo que ele afirma.

Leia toda saída você mesmo antes de aceitar o veredito. Se discordar do juiz, desconfie da rubrica antes de desconfiar do seu julgamento.

## Construa uma UI de bot com `/make-bot-ui`

Uma skill situacional é para usuários do Grok Bot. O [`/make-bot-ui`](../../.github/skills/make-bot-ui/SKILL.md) constrói uma página pequena cujos botões acordam um bot por uma rotina de webhook. Por exemplo, você pode deslizar por uma fila de revisão e fazer cada deslize pedir ao bot para agir naquele item. Um servidor na sua máquina guarda a chave de envio do webhook, então a chave nunca chega ao browser nem ao chat. A skill também cobre expor a página no Tailscale.

**Armadilha.** Não edite uma skill no meio da tarefa porque ela está se comportando mal. Corrija num PR próprio e mantenha a tarefa andando. Uma edição de skill que sai emaranhada em trabalho de funcionalidade fica invisível na revisão e impossível de avaliar.

A seguir, [Receitas e armadilhas](./10-recipes-and-pitfalls.md).
