# Triagem de Bugbot

Use esta referência quando o playbook Babysit (`../playbooks/babysit.md`) trata comentários do Bugbot ou de automação de revisão. O objetivo não é ignorar o Bugbot por padrão. O objetivo é parar de tratar todo comentário como uma mudança de código obrigatória.

## Régua de decisão

Classifique cada thread do Bugbot antes de agir:

- `fix`: o comentário aponta um problema plausível de correção, segurança, privacidade, perda de dados, autenticação, cobrança, migração, idempotência, corrida ou comportamento entregue. Corrija na PR dona mais baixa, depois responda com o SHA do commit e resolva a thread.
- `dismiss`: o comentário casa com um padrão barulhento de baixo risco já documentado, e o código ou contexto atual provam que a preocupação não exige mudança de código. Responda com um motivo curto e resolva a thread.
- `ask`: o comentário é novo, de alta severidade, ligado a segurança, privacidade ou dados, ou ambíguo. Pergunte ao usuário em vez de adivinhar.

Na dúvida, pergunte. Pular um comentário barulhento de qualidade de código é barato. Pular um bug real de dados ou segurança não é.

## Formato de padrão aprendido

Acrescente padrões futuros neste formato:

```markdown
### <nome curto do padrão>

- Confidence: candidate | recurring | strong
- Skip when: <condições que precisam ser verdadeiras>
- Do not skip when: <fronteiras de risco>
- Example signal: <frases ou contexto de código que identificam o padrão>
- Source: <URL da PR/comentário ou nota histórica curta>
```

Use `candidate` para um ou dois exemplos. Use `recurring` depois de várias dispensas reais. Use `strong` só quando o padrão é estreito, verificado repetidamente e de baixo risco.

## Candidatos recorrentes de dispensa

### Mudanças visuais intencionais de UI ou design system

- Confidence: candidate
- Skip when: a descrição da PR, capturas de tela, revisão de design ou código próximo tornam a mudança visual explícita, e o comentário do Bugbot só repete que um padrão visual compartilhado mudou.
- Do not skip when: o comentário aponta acessibilidade, visibilidade de foco, navegação por teclado, contraste de cor, ou um contrato de API de componente que a PR não mudou de propósito.
- Example signal: comentários sobre contornos de foco, tamanhos de botão, espaçamento, ou padrões visuais padrão de componente compartilhado em que o dono responde "intentional" ou "intended".

### Uso no topo da pilha ou local que o Bugbot não enxerga

- Confidence: candidate
- Skip when: o Bugbot marca um export, componente, helper ou arquivo como não usado, e a lista de PRs e diffs do forge ativo, diffs do topo da pilha, ou contexto da PR mostram que uma PR posterior o usa.
- Do not skip when: a PR atual não faz parte de uma pilha, o símbolo é API pública, ou o suposto uso no topo da pilha não pode ser verificado.
- Example signal: "Exported component is never used" com uma resposta humana do tipo "used upstack".

### Duplicação temporária durante implementação em paralelo

- Confidence: candidate
- Skip when: a PR duplica de propósito um pouco de código para manter um caminho novo em paralelo com um caminho antigo que está sendo apagado, substituído ou comprovado.
- Do not skip when: o código duplicado mexe em segurança, cobrança, acesso a dados, comportamento de API, ou uma abstração compartilhada de vida longa claramente reduziria o risco.
- Example signal: "Significant duplication" ou "duplicated validation logic" em que o dono explica que o caminho antigo será apagado ou que a lógica duplicada é local de propósito.

### Invariante de framework ou componente já cobre o aviso

- Confidence: candidate
- Skip when: a preocupação já é garantida por um componente compartilhado, contrato de framework, invariante de tipo, ou fonte única de verdade visível no diff atual ou no código próximo.
- Do not skip when: o invariante é suposto mas não imposto, depende de timing, ou cruza fronteiras de async e estado onde os valores podem divergir.
- Example signal: comentários sobre max-height ausente num popover interno quando o popover compartilhado impõe limites de viewport, ou valores anuláveis em que o valor checado local e o valor passado vêm da mesma fonte.

### Follow-up declarado pelo dono ou limpeza adiada

- Confidence: candidate
- Skip when: o dono da PR diz explicitamente que o problema é um follow-up conhecido, o comportamento não piora com a PR atual, e o comentário não é sobre área de alto risco.
- Do not skip when: o agente age sem a opinião do dono, o problema é comportamento de produto de severidade média ou alta, ou adiar fundiria uma regressão nova.
- Example signal: "I'll worry about that later" ou "we'll delete this eventually".

### Comentários de regra auto-retirados ou explicitamente falso-positivo

- Confidence: recurring
- Skip when: o corpo do comentário ou uma resposta posterior do Bugbot diz explicitamente que o achado foi retirado, está em conformidade, ou é falso positivo, e o agente consegue verificar a regra localmente.
- Do not skip when: a única evidência é um humano dizendo "false positive" num problema de alto risco sem explicação.
- Example signal: um comentário de regra de nome de arquivo cujo corpo diz que o arquivo já está em conformidade.

## Pergunte por padrão

Não pule automaticamente estas categorias, mesmo que uma PR anterior tenha dispensado algo parecido:

- Achados de segurança, privacidade, autenticação, cobrança, retenção de dados, dados de treino, e de fronteira de permissão.
- Achados de alta severidade.
- Achados de migração, schema, idempotência, concorrência e comportamento entre sistemas.
- Comentários em que a correção sugerida é pequena e claramente reduz risco sem mudar a intenção do produto.

Dados históricos mostraram humanos dispensando comentários de segurança ou fluxo de dados. Trate esses casos como decisão do dono, não como regra de dispensa do time.

## Aprendizados candidatos de babysits recentes

Acrescente aprendizados candidatos aqui durante ou depois do babysit, quando parecerem úteis ao time mas ainda não maduros. Prefira promover candidatos recorrentes para a seção acima quando várias PRs confirmarem o padrão.

### Reimplementações manuais de comportamento nativo do navegador

- Confidence: candidate
- Skip when: praticamente nunca. Quando um diff troca comportamento nativo do navegador por um equivalente manual (sticky nativo → clones posicionados por JS, mira de scroll nativa → eventos de wheel e touch repassados, oclusão por paint-order → máscaras e clip-path), os achados de bug de lógica do Bugbot contra esse código têm sido consistentemente legítimos.
- Do not skip when: o achado trata de lacunas no repasse de eventos (deltaMode do wheel, pans de touch, encadeamento de scroll nas bordas, tap slop), divergência de hit-testing de máscara e clip, ou corridas de timing entre observer e estado do React nesse tipo de código. O padrão é corrigir.
- Example signal: "masks do not affect hit-testing", "overlay blocks wheel scroll", "ignores deltaMode", "runs in the IntersectionObserver callback before React applies state".
- Source: uma PR de oclusão de sticky, seis passadas do Bugbot, cerca de dezoito achados, todos corrigidos em vez de dispensados.

### Alegações de desvio de teste de contrato são baratas de verificar, rode o teste primeiro

- Confidence: candidate
- Skip when: nunca pule a verificação em si, ela custa um comando. Quando uma PR
  entrega um teste de contrato que prende prosa de protocolo ou de documentação
  (regexes sobre um SKILL.md, snapshot do texto da doc), e o Bugbot alega que "o
  teste não casa mais com a doc" (ou o contrário), rode esse teste na ponta da PR
  antes de classificar. Uma execução vermelha confirma a alegação. Uma execução
  verde é refutação concreta para a resposta de dispensa.
- Do not skip when: n/a, este é um atalho de verificação, não um padrão de
  dispensa. Heurísticas de dispensa por passadas repetidas erram aqui:
  testes que prendem prosa se desviam exatamente PORQUE rodadas de correção
  anteriores editam a prosa.
- Example signal: "Contract test omits the pre-fix wait" numa PR cujos commits de
  correção anteriores reescreveram a passagem presa. O teste rodado na ponta
  falhou exatamente na asserção citada.
- Source: uma PR que prende prosa, com oito passadas do Bugbot. A alegação era
  real na passada 7, apesar de toda passada anterior ter sido corrigida e resolvida.

### Achado de revisão de segurança obsoleto, já corrigido mais tarde na mesma PR

- Confidence: candidate
- Skip when: uma revisão de segurança agêntica (ou parecida) alega uma chamada de autorização ou validação ausente, e a ponta atual da PR inclui claramente exatamente essa guarda (com testes), normalmente acrescentada num commit de endurecimento posterior à revisão.
- Do not skip when: o helper citado é no-op para o principal em discussão, a checagem roda depois do efeito colateral que ela protege, ou falta cobertura para o principal alegado.
- Example signal: um achado HIGH de "missing authorization check" enquanto a guarda exata já é chamada antes do efeito colateral na ponta.
- Source: uma PR de endpoint de webhook cujo commit de endurecimento veio depois da revisão.

### Alargar uma condição de erro deliberadamente estreita esconderia o erro real

- Confidence: candidate
- Skip when: o achado pede para alargar uma condição de erro estreita (um
  `errno` específico, código de erro, ou classe de status) para um catch-all, e
  essa estreiteza codifica uma distinção real. A forma canônica é um fallback de
  dependência condicionado a `ENOENT`, "binário não instalado" é situação
  diferente de "o comando rodou e falhou". Retentar em qualquer saída não-zero
  rodaria uma falha legítima (não encontrado, autenticação expirada, rede)
  contra o fallback e depois relataria o erro do fallback, escondendo o verdadeiro.
- Do not skip when: a condição estreita perde um caso da MESMA categoria (outro
  errno de "binário inutilizável" como `EACCES`, outra falha em nível de
  transporte), o caminho não tratado perde dados ou deixa estado parcial, ou a
  retentativa é idempotente E o erro original continua visível.
- Example signal: "only retries when X fails with ENOENT … never tries the
  fallback even when a working Y exists", apontando para código cujo fallback
  existe para uma dependência ausente e não para uma operação que falhou.
- Source: uma PR de renomeação de CLI cujo fallback existia para um binário
  ausente e não para um comando que falhou.
