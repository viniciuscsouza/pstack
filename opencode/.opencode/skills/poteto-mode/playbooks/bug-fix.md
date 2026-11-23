### Bug fix

**A tarefa é sua. Planeje, revise, verifique.** Delegue a investigação e a correção aos subagentes, fique na liderança. Escolha o nível de raciocínio de cada subagente conforme a seção Subagentes.

Seja científico. Toda linha entregue rastreia até evidência de runtime. Cinto e suspensório que "pode ajudar" é hipótese, não correção. Não entra. Quando a evidência refuta uma hipótese, reverta o que ela motivou. Entra a menor mudança que a evidência justifica, nada além.

1. Reproduza você mesmo na superfície correspondente pela skill de controle (Inegociáveis), mesmo quando um protocolo de debug ou instrumentação mandar pedir ao usuário para reproduzir. Pergunte ao usuário só com uma razão específica e declarada de que a superfície de controle não alcança o alvo, e só depois de conduzir ela até o limite. Se não reproduzir direto, sintetize o gatilho, aperte as condições, ou instrumente até disparar.
2. Faça busca binária na causa. Formule as hipóteses candidatas, depois elimine até sobrar uma. Alimente elas com um trace do subsistema afetado e a skill **why** para histórico de regressão. A cada passada, pegue a divisão que corta o maior espaço de problema restante, obtenha evidência de runtime, elimine. Quando o estado do programa não estiver claro, adicione instrumentação ou log e leia conforme o código roda. Não chute. Conduza uma caçada longa ou teimosa com o comando `/loop` do Cursor. Confirme o *mecanismo* sobrevivente com evidência de runtime antes do `architect` do passo 3 ou de um leque `interrogate`.
3. Planeje a correção. Se ela cruza uma fronteira de função, `architect` primeiro. Delegue a implementação a um subagente usando o seu modelo de bug-fix configurado (padrão `claude-opus-5-5-xhigh-fast`) com escopo específico.
4. Verifique na mesma superfície. O repro original agora passa. "Inconclusivo" ou superfície errada não é aprovação. Sinalize. Testes unitários mostram comportamento de ramo, não ausência de bug.
5. Ordene os commits para o repro que falha aterrissar antes da correção no histórico do git. Veja a skill **tdd** para a cadência de teste que falha primeiro quando o bug tem um caminho de teste local barato. Pule quando o teste for caro, pesado em integração ou pouco claro.
   Esta é a skill de princípio **sequence-verifiable-units** canônica, o teste que falha primeiro e a correção em cima.
6. Rode **Opening a PR**.

**Resposta:** o que estava quebrado, causa raiz, correção, como você verificou. Cole a saída do repro falhando e depois passando, literal.
