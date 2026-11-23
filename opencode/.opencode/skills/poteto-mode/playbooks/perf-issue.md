### Perf issue

**Você é dono da história da medição. Planeje, revise, verifique os números.** Amarre toda correção a uma medição, não leia o código-fonte no lugar de medir.

1. Capture um trace de linha de base via a skill de controle correspondente. Valide a linha de base, e cada número posterior, com a skill **benchmark-checklist**.
2. Rastreie o código para fundamentar hipóteses. Não afirme um teto de desempenho sem rodar primeiro.
   Tente os mantras de performance em ordem, do mais barato primeiro:
   1. Não faça. Pare trabalho cujo resultado nada usa em vez de barateá-lo.
   2. Faça, mas não faça de novo.
   3. Faça menos.
   4. Faça depois.
   5. Faça quando ninguém está olhando.
   6. Faça em paralelo.
   7. Faça mais barato.

   Quando um mantra anterior atinge o alvo, pare.
3. Planeje a correção a partir do trace. Se ela atravessa uma fronteira de função, `architect` primeiro. Delegue a implementação a um subagente usando seu modelo de perf-issue configurado (padrão `claude-opus-5-5-xhigh-fast`). Escolha o nível de raciocínio de cada subagente para o trabalho, conforme a seção Subagentes. Revise o diff. Capture um trace pós-correção.
   Aplique a skill do princípio **sequence-verifiable-units**, verificando cada tentativa antes de tentar a próxima.
4. Faça parse e compare os artefatos (JSON para sqlite, diff). "Inconclusivo" ou superfície errada não é aprovação. Sinalize.
5. Cite a medição na PR.
6. Rode **Opening a PR**.

Para melhoria sustentada contra uma métrica em vez de uma correção única, use o playbook Hillclimb (`playbooks/hillclimb.md`).

**Resposta:** número da linha de base, número pós-correção, delta, caminho do artefato.
