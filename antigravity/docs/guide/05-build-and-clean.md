# Construa a mudança e limpe o diff

Os playbooks de construção compartilham uma disciplina. Diga o que você observou, e deixe o playbook exigir a evidência. Esta página mostra o que pôr no prompt para cada tarefa de construção comum, depois o hábito de limpeza que mantém os diffs revisáveis.

## Alimente cada playbook de construção com o que você sabe

Um prompt de bug declara o sintoma e pede uma reprodução primeiro:

```text
/poteto-mode this command emits two records after a retry. repro first, then fix and verify.
```

Um prompt de feature declara o comportamento e o que não pode mudar:

```text
/poteto-mode add a --json flag. text output stays byte-identical. verify both forms.
```

Um prompt de refatoração prende o comportamento antes de a estrutura mexer:

```text
/poteto-mode move parsing into one module, zero behavior change. record the current output first and prove it's unchanged after.
```

Um prompt de perf declara a medição, não uma impressão:

```text
/poteto-mode startup takes 1.8s on this fixture. trace it, fix the measured cause, show me before and after.
```

Cada um destes roteia para o seu playbook ([Bug fix](../../.agents/skills/poteto-mode/playbooks/bug-fix.md), [Feature](../../.agents/skills/poteto-mode/playbooks/feature.md), [Refactoring](../../.agents/skills/poteto-mode/playbooks/refactoring.md), [Perf issue](../../.agents/skills/poteto-mode/playbooks/perf-issue.md)), e o playbook fornece os passos que você não digitou. Reproduzir antes de corrigir, nomear a forma dos dados antes de implementar, prender o comportamento antes de reestruturar, perfilar antes de otimizar.

Para melhoria sustentada de um número, existe o [Hillclimb playbook](../../.agents/skills/poteto-mode/playbooks/hillclimb.md). Dê a métrica, um alvo, e um piso de tentativas, e ele roda um laço de uma hipótese por vez com um harness de medição congelado. Ele mantém os ganhos e reverte todo o resto.

Os dois playbooks de perf rodam [`/benchmark-checklist`](../../.agents/skills/benchmark-checklist/SKILL.md) nos seus números. Perf issue valida a linha de base e cada número depois dela, e Hillclimb valida o harness antes de congelar. [Verifique e entregue](./06-verify-and-ship.md#vet-a-measured-number-with-benchmark-checklist) mostra quando digitar você mesmo.

Às vezes você quer a causa antes de qualquer correção. Para um sintoma ao vivo, como um vazamento, um giro de CPU ociosa, ou um glitch visual, o [Runtime forensics playbook](../../.agents/skills/poteto-mode/playbooks/runtime-forensics.md) instrumenta o processo em execução. Para um profile já capturado, o [Trace forensics playbook](../../.agents/skills/poteto-mode/playbooks/trace-forensics.md) lê o artefato e mapeia o frame quente para o código-fonte. Os dois devolvem um diagnóstico, não uma correção:

```text
/poteto-mode here's a cpuprofile from the slow startup. tell me where the time goes and which source lines own it. no fix yet.
```

## Escreva o teste que falha primeiro com `/tdd`

Quando um bug tem um caminho de teste local barato, o prompt inteiro pode ser duas palavras:

```text
/tdd implement
```

Em contexto, isso basta. [`/tdd`](../../.agents/skills/tdd/SKILL.md) escreve o menor teste que falha pelo motivo pretendido, depois a correção, depois roda o teste de novo. Se um teste precisaria de montagem ampla de harness ou mocks frágeis, a skill diz isso e usa a checagem executável mais próxima. Não force um teste onde um comando real é evidência mais forte.

## Carregue as regras de TypeScript pelo nome

[`typescript-best-practices`](../../.agents/skills/typescript-best-practices/SKILL.md) transforma os princípios de sistema de tipos em regras concretas. Uniões discriminadas, `unknown` nas fronteiras, variantes exaustivas, tipos derivados de schema. Ela não carrega sozinha, então digite `/typescript-best-practices` quando uma tarefa toca arquivos `.ts` ou `.tsx`.

## Limpe antes de commitar

O [Opening a PR playbook](../../.agents/skills/poteto-mode/playbooks/opening-a-pr.md) roda `/deslop` no diff antes de cada commit, e o poteto-mode aplica [`/unslop`](../../.agents/skills/unslop/SKILL.md) na descrição do PR e nos corpos dos commits. O `/deslop` vem no plugin `cursor-team-kit`, não no pstack. Se você não tem, peça o mesmo resultado em palavras simples. Remova comentários narrando, guardas sem sustentação, caminhos de compatibilidade mortos, e edições sem relação.

Para prosa, `/unslop` recebe um alvo e quaisquer regras extras que você tenha:

```text
/unslop the readme changes, no emdashes
```

Você vai desenvolver seu próprio atalho. A skill lê a intenção bem de prompts telegráficos como `unslop that, tighten it`.

## Arranque os comentários com `/no-comments`

Comentários precisam de uma passada própria, e não do agente que os escreveu. Um autor defende os comentários dele como você defenderia os seus. Então antes da revisão, entregue a olhos novos:

```text
/no-comments the diff
```

[`/no-comments`](../../.agents/skills/no-comments/SKILL.md) gera o [Comment Sicko](../../.agents/agents/comment-sicko.md), um revisor somente leitura com uma lista curta do que fica. Cabeçalhos de licença, doc comments numa API pública, links que explicam o que o código não consegue, comportamento forçado por uma dependência externa que você não pode remodelar. Todo o resto sai. Uma surpresa no seu próprio código não ganha essa passada. O comentário volta como flag de refatoração, e `/no-comments` corrige as flags que aceita na causa raiz. Quando um comentário alega uma restrição, "do not remove", a skill oferece codificar a alegação como tipo, teste ou lint. De qualquer jeito, o comentário sai.

A divisão de trabalho vale manter clara. `/deslop` limpa o slop do código, `/unslop` limpa o slop da prosa, e `/no-comments` entrega os comentários a um revisor que não os escreveu.

**Armadilha:** limpeza não é polimento opcional. Um diff com comentários narrando e peso morto defensivo lê como inacabado para revisores, e o código extra é onde o próximo bug se esconde. Se o diff parece inflado, diga `deslop it` antes de commitar, não depois que a revisão apontar.

Próximo: [Verifique e entregue](./06-verify-and-ship.md).
