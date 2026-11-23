### Autonomous run

**Você é dono da condição de saída. Defina pronto e conduza até lá sem parar.**

1. Enuncie a condição de saída como um predicado verificável antes da primeira iteração (testes verdes, repro corrigida, todos os N PRs mergeados, pixel-diff zero).
2. Escolha o mecanismo de despertar usando o comando `/loop` do Cursor (nativo, não uma skill do pstack). Um evento a observar (CI, um merge, uma ref avançando) ganha um subagente observador que desperta você no evento, com um heartbeat longo por tempo como fallback. Sem evento, o heartbeat tem intervalo fixo, dimensionado para quando vale rechecar o resultado. Escolha o nível de raciocínio de cada subagente para o trabalho, conforme a seção Subagentes.
3. Cada iteração faz a menor mudança que a evidência justifica, verifica contra o predicado, commita se avançou, descarta mudanças que não ajudaram. Redundância defensiva que "talvez ajude" é revertida, não fica no código.
   Sequencie o trabalho pela skill de princípio **sequence-verifiable-units**, verificando cada unidade antes da próxima em vez de agrupar checagens no fim.
4. Descobertas no meio da execução que apoiam o objetivo são suas. Trate skills quebradas, bugs relacionados, verificadores instáveis, ruído de revisão, falhas de ferramenta, follow-ups órfãos e desvio corrigível você mesmo via poteto-mode. Ponha correções fora de faixa no PR delas. Não estacione trabalho reversível para o humano nem use `AskQuestion`. Exponha só ações irreversíveis, decisões reais de produto ou preferência que nenhum experimento resolve, ou um beco sem saída de verdade. Mantenha o predicado como o motor principal, e volte a ele depois de cada correção lateral.
5. Registre um checkpoint a cada iteração pela skill **show-me-your-work**, uma linha para o que mudou e se o predicado andou.
6. Pare quando o predicado é atendido. Um platô não é parada, então continue e mude a abordagem para passar dele. Exponha um beco sem saída de verdade em vez de girar em falso, e nunca relaxe o predicado para declarar vitória.

**Resposta:** a condição de saída, iterações rodadas, o que aterrissou, o que foi descartado, estado final do predicado.
