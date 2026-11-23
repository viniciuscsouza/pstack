---
name: swarm
description: "Abra N workers em paralelo, drene todos e devolva um relatório. Use para /swarm, 'swarm nisto', ou cobertura paralela, corridas, gauntlets e exploração."
disable-model-invocation: true
---

# Swarm

## Início

Abra uma lista de tarefas com uma entrada por fase antes de disparar qualquer coisa.

1. Enquadrar
2. Abrir em leque
3. Agregar
4. Relatar

## Fase A: Enquadrar

1. Declare o predicado de concluído e o artefato ou relatório que o swarm precisa devolver.
2. Escolha a forma. Parta em fatias, corra N workers em briefings idênticos, ou misture as duas. Para uma corrida ou uma forma mista, declare `first pass`, `rank all` ou `best-of` antes de disparar.
3. Escolha o modelo dos workers na linha `swarm workers` de `~/.cursor/rules/pstack-models.mdc`. Se a regra ou essa linha faltar, use `claude-opus-5-5-high-fast`. Para `auto` ou `inherit-parent`, omita `model` para os workers rodarem no modelo pai. Se a ferramenta Task rejeitar um slug, use o padrão e diga isso. Se ela rejeitar o padrão, use o slug válido mais próximo da mesma família a partir da mensagem de erro. Para uma corrida de modelos, nomeie o modelo de cada braço de antemão. Escolha o nível de raciocínio de cada subagente para o trabalho, conforme a seção Subagentes da skill **poteto-mode**.
4. Dê a cada worker a própria saída gravável quando ele escreve. Quando workers verificam ou medem commits, cada briefing nomeia os SHAs exatos. Um briefing de medição também nomeia o método (tamanho da amostra, o que é uma amostra, ordem). O worker registra os dois no resultado.

## Fase B: Abrir em leque

Crie todos os N workers numa única mensagem com `subagent_type: generalPurpose`, `environment: "cloud"`, `run_in_background: true`, e o modelo do passo 3, deixado sem definir para `auto` ou `inherit-parent`. Use `environment: "local"` só quando o worker precisa de acesso a algo no computador do usuário.

Quando um worker precisa começar de um branch empurrado não padrão, passe `cloud_base_branch`.

Todo briefing é autônomo. Inclua o objetivo, o escopo, a fatia ou o braço exato, como verificar, e o que relatar. Relatórios usam `PASS`, `ISSUES` ou `BLOCKED` com evidência. Um worker que consegue provar um defeito relata `ISSUES` e lista todo problema que consegue provar, não só o primeiro.

Se um worker cair fora, siga com N-1 e registre isso.

## Fase C: Agregar

Leia os resultados de terminal. Descarte um resultado que não registra os SHAs e o método que o briefing dele nomeia, e recrie esse worker uma vez. Depois de uma segunda falta, registre uma lacuna. Uma lacuna não conta como aprovação. Para cobertura, toda fatia exigida precisa de um resultado. Para uma corrida, aplique a regra de seleção declarada de antemão. Use first pass, rank all ou best-of. Não cole despejos brutos de worker.

## Fase D: Relatar

Devolva um relatório consolidado no chat com uma tabela de resultados, problemas em uma linha cada, lacunas ou desistências, e a regra da corrida quando usada.
