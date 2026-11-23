# Direcione com nomes de princípios

O pstack entrega 24 princípios como skills individuais. O `/poteto-mode` lê o índice deles no começo de toda tarefa de várias etapas, aplica os que a tarefa dispara, e nomeia cada princípio aplicado na resposta junto com a decisão que ele mudou.

Você não invoca princípios. Você usa os nomes deles para direcionar. Cada nome aponta para uma regra completa que o agente já leu, então uma frase redireciona o trabalho com mais precisão que um parágrafo de instruções.

## Direcionar na prática

Digamos que o agente esteja prestes a parafusar um adaptador novo em três que já existem.

```text
use subtract before you add. delete the obsolete adapters first, then design what's left.
```

Digamos que ele afirme sucesso porque o build passou.

```text
apply prove it works. run the real import flow and show me the written records.
```

Digamos que duas tentativas paralelas estejam prestes a escrever no mesmo branch.

```text
separate before serializing shared state. give each attempt its own worktree, no locks.
```

Cada frase acerta porque a regra por trás dela é específica. O agente ainda precisa dizer, na resposta, qual decisão a regra mudou. Uma citação de princípio sem decisão por trás significa que o agente nomeou a regra sem aplicá-la.

## Os 24, em resumo

Os princípios de núcleo decidem quanto construir e quando repensar o design.

- [Protocolo da Preguiça](../../.agents/skills/principle-laziness-protocol/SKILL.md) prefere a deleção e a menor mudança que resolve o problema.
- [Pensamento Fundacional](../../.agents/skills/principle-foundational-thinking/SKILL.md) escolhe as estruturas de dados centrais antes de escrever lógica.
- [Redesenhe a Partir dos Primeiros Princípios](../../.agents/skills/principle-redesign-from-first-principles/SKILL.md) integra um requisito novo como se ele estivesse lá desde o primeiro dia.
- [Ataque a Premissa](../../.agents/skills/principle-attack-the-premise/SKILL.md) escreve a premissa que duas ou mais correções falhas compartilharam e testa ela direto antes da próxima correção.
- [Subtraia Antes de Adicionar](../../.agents/skills/principle-subtract-before-you-add/SKILL.md) remove o peso morto antes de construir em cima dele.
- [Minimize a Carga do Leitor](../../.agents/skills/principle-minimize-reader-load/SKILL.md) colapsa camadas e estado escondido que um leitor precisa segurar na cabeça.
- [Execução Orientada a Resultado](../../.agents/skills/principle-outcome-oriented-execution/SKILL.md) converge reescritas no design alvo em vez de preservar estados de compatibilidade descartáveis.
- [Experiência Primeiro](../../.agents/skills/principle-experience-first/SKILL.md) escolhe o resultado do usuário acima da conveniência de implementação.
- [Esmigalhe o Espaço de Design](../../.agents/skills/principle-exhaust-the-design-space/SKILL.md) constrói dois ou três protótipos concorrentes quando não há precedente.
- [Construa a Alavanca](../../.agents/skills/principle-build-the-lever/SKILL.md) constrói o script que faz ou prova o trabalho, para um revisor poder rodá-lo de novo. Quando um agente segue fazendo a mesma coisa na mão, mande ele escrever a ferramenta ou skill que gostaria de ter. Se um script consegue fazer um passo do mesmo jeito toda vez, use o script, e guarde os agentes para as decisões de julgamento.

Os princípios de arquitetura decidem onde vivem estado, validação e compatibilidade.

- [Modele o Domínio](../../.agents/skills/principle-model-the-domain/SKILL.md) codifica regras repetidas numa estrutura, não em condicionais espalhados.
- [Disciplina de Fronteira](../../.agents/skills/principle-boundary-discipline/SKILL.md) valida na fronteira e confia nos tipos internos.
- [Disciplina de Sistema de Tipos](../../.agents/skills/principle-type-system-discipline/SKILL.md) torna estados ilegais irrepresentáveis.
- [Torne Operações Idempotentes](../../.agents/skills/principle-make-operations-idempotent/SKILL.md) converge retentativas no mesmo estado final.
- [Migre os Chamadores, Depois Apague as APIs Legadas](../../.agents/skills/principle-migrate-callers-then-delete-legacy-apis/SKILL.md) migra e apaga numa única onda.
- [Separe Antes de Serializar Estado Compartilhado](../../.agents/skills/principle-separate-before-serializing-shared-state/SKILL.md) remove o compartilhamento antes de adicionar coordenação.

Os princípios de verificação definem o que conta como prova.

- [Prove que Funciona](../../.agents/skills/principle-prove-it-works/SKILL.md) verifica o artefato real, não um proxy.
- [Corrija a Causa Raiz](../../.agents/skills/principle-fix-root-causes/SKILL.md) reproduz e rastreia até a causa antes de mudar código.
- [Sequencie o Trabalho em Unidades Verificáveis](../../.agents/skills/principle-sequence-verifiable-units/SKILL.md) termina cada unidade pequena numa checagem antes de começar a próxima.
- [Teste Comportamento, Não Implementação](../../.agents/skills/principle-test-behavior-not-implementation/SKILL.md) afirma o que os usuários do código podem observar, para um teste falhar quando esse comportamento quebra e sobreviver a uma mudança no interno.
- [Explique o Número](../../.agents/skills/principle-explain-the-number/SKILL.md) nomeia o que limita um número medido e descarta que ele mediu outra coisa, antes de alguém confiar ou relatar. O [`/benchmark-checklist`](../../.agents/skills/benchmark-checklist/SKILL.md) transforma isso em sete perguntas que você responde a partir de execuções reais.

Os princípios de delegação mantêm o trabalho paralelo sadio.

- [Proteja a Janela de Contexto](../../.agents/skills/principle-guard-the-context-window/SKILL.md) roteia a leitura em volume para subagentes e mantém os achados no chat principal.
- [Nunca Bloqueie no Humano](../../.agents/skills/principle-never-block-on-the-human/SKILL.md) segue em trabalho reversível e apresenta o resultado.

E um princípio meta.

- [Codifique Lições em Estrutura](../../.agents/skills/principle-encode-lessons-in-structure/SKILL.md) transforma um conselho que você repetiu duas vezes em lint, checagem ou script. O [`/correct`](../../.agents/skills/correct/SKILL.md) aplica isso a um repositório inteiro, como mostrado em [Faça do seu jeito](./09-make-it-yours.md#fix-the-environment-with-correct).

Não memorize a lista. Passe os olhos agora, e volte quando pegar o agente fazendo algo que um nome aqui teria evitado. É assim que o vocabulário gruda.

A seguir, [Faça do seu jeito](./09-make-it-yours.md).
