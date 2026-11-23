---
name: setup-pstack
description: Configure quais modelos o pstack usa por papel e com qual orçamento de raciocínio. Detecta os modelos disponíveis e escreve uma regra sempre aplicada que sobrepõe os padrões das skills. Use para /setup-pstack, "configurar modelos do pstack", "orçamento do pstack", ou para trocar as escolhas de modelo do pstack.
---

# Configurar o pstack

Escreva `~/.cursor/rules/pstack-models.mdc`, uma regra sempre aplicada que define o modelo do pstack por papel.

## Passos

### 1. Detecte os modelos disponíveis

Enumere os slugs de modelo que você consegue passar a um subagente `Task` nesta sessão. Essa é a fonte confiável. Se o Cursor também expõe uma API de modelos ou uma CLI que lista os modelos a que o usuário tem direito, prefira ela por completude. Se você não conseguir detectar nenhum, peça ao usuário para colar os slugs aos quais ele tem acesso. Nunca escreva um slug real que você não confirmou estar disponível. Os aliases `inherit-parent` e `auto` sempre valem, mesmo não sendo slugs detectados.

### 2. Carregue o estado atual

O mapeamento padrão de papel para modelo é a forma de regra mostrada no passo 5 abaixo. Se `~/.cursor/rules/pstack-models.mdc` já existe, leia o arquivo e trate a linha `# budget` e os valores de papel dele como as escolhas atuais. Caso contrário, comece por esses padrões. Uma linha cujo papel não está no passo 5, como `hardest tasks`, `architect runners`, `reflect tooling` ou `why investigators`, vem de um papel aposentado. Descarte.

Uma regra sem a linha `# defaults: 3` vem de uma configuração antiga. Essa configuração escrevia uma linha para cada papel, então alguns valores dela são padrões antigos, não escolhas. Antes do passo 3, substitua estes valores:

- Uma lista de `arena cross-judge pool` com exatamente uma entrada `claude-fable-*`, uma entrada `claude-opus-*` e uma entrada `gemini-*`, em qualquer ordem, recebe a lista do passo 5 do papel. Fable e Opus são um só fornecedor, então a entrada Fable só poderia julgar onde a entrada Opus também poderia.
- Uma linha `# budget: unlimited` vira `large`, porque nenhum papel roda acima de `xhigh`.
- Se a regra também não tem a linha `# defaults: 2`, um slug antigo é o que casa com `grok-*`. Um slug antigo em `feature, refactoring`, `bug-fix`, `perf-issue`, `hillclimb` ou `swarm workers` recebe o padrão do passo 5 do papel. Num papel de painel (`arena runners`, `arena cross-judge pool`, `interrogate reviewers`), uma lista com exatamente uma entrada `claude-opus-*` e um slug antigo, em qualquer ordem, recebe a lista do passo 5 do papel. Em qualquer outra lista, cada slug antigo vira `claude-opus-5-5-high-fast`.

Todo outro valor é escolha do usuário e fica, inclusive um slug antigo em qualquer outro papel e todo valor `inherit-parent` ou `auto`. Diga ao usuário quais valores você substituiu.

### 3. Orçamento, mapa e confirmação

**(a) Peça um orçamento.** Prefira AskQuestion a texto livre. Ofereça estas três opções com estes rótulos exatos, e nomeie o orçamento atual quando a regra registra um. Sem regra, diga que `large` corresponde aos padrões das skills.

- `large — xhigh reasoning`
- `medium — high reasoning`
- `small — medium reasoning`

**(b) Aplique.** Monte a tabela de trabalho a partir dos padrões das skills, e numa nova execução mantenha todo papel que você mudou por família, lista ou alias (`inherit-parent`, `auto`). As famílias são as que a Fase A da skill **arena** define, então Fable é família própria. Cada papel tem um esforço alvo, que é o esforço do padrão dele movido pelo orçamento. `large` mantém, `medium` desce um passo, e `small` desce dois passos, na escada `xhigh` > `high` > `medium` > `low`. Nenhum papel roda acima de `xhigh`, e um alvo no fundo da escada fica lá. A maioria dos padrões está em `xhigh`, e cada rótulo de orçamento nomeia onde eles caem. Defina o token de esforço de todo slug real do papel, entradas de painel incluídas, para o alvo do papel. O token de esforço é o último token, ou o anterior a um `fast` no fim. Se o resultado não for um slug detectado, use o slug detectado da mesma família com o esforço mais alto igual ou abaixo do alvo, senão marque o papel como precisando de escolha. `inherit-parent` e `auto` não mudam. Então `medium` transforma `claude-opus-5-5-xhigh-fast` em `claude-opus-5-5-high-fast` e `claude-opus-5-5-high-fast` em `claude-opus-5-5-medium-fast`, e `small` transforma os dois em `claude-opus-5-5-medium-fast` e `claude-opus-5-5-low-fast`. Opus 5.5 sempre roda com o slug `-fast`, então todo valor Opus 5.5 termina em `-fast`. Fable 5.2 e Gemini não têm slug fast, então nunca escreva um. Gemini para em `high`, então uma entrada Gemini roda em `high` quando o alvo do papel dela é `xhigh`.

**(c) Mostre os papéis e confirme.** Mostre todo papel com o modelo dele, marcando todo slug real fora do conjunto detectado como precisando de escolha. Liste também cada linha que o passo 2 descartou. Pergunte se aceita como está ou se muda papéis específicos, oferecendo os modelos detectados em `xhigh` ou abaixo mais `inherit-parent` e `auto` (os dois significam que este papel roda no modelo do chat pai, que é como usuários do Auto continuam no Auto) como opções. Prefira AskQuestion a texto livre. Para papéis de painel (arena runners, interrogate reviewers) o valor é uma lista, e um subagente roda por entrada, entradas de alias incluídas, então o tamanho da lista define a contagem. `arena cross-judge pool` também é uma lista, mas a Arena roda um juiz. Ela tenta só valores de um fornecedor diferente do pai, na ordem da lista, então ordene por preferência. Fable e os outros modelos Claude são um só fornecedor. Um valor `inherit-parent` ou `auto` nunca julga. `swarm workers` é o modelo padrão de todo worker, a menos que uma corrida ou comparação atribua outro modelo por braço.

### 4. Valide

Todo slug real escrito precisa estar no conjunto detectado e em `xhigh` ou abaixo. `inherit-parent` e `auto` sempre passam. Se um slug real escolhido não estiver disponível, pare e pergunte de novo.

### 5. Escreva a regra

Escreva `~/.cursor/rules/pstack-models.mdc` com `alwaysApply: true`, a linha `# defaults: 3`, uma linha `# budget` com o rótulo escolhido e o esforço que o rótulo nomeia, e uma linha por papel, usando os mesmos rótulos que o poteto-mode usa. Sobrescreva o arquivo inteiro para novas execuções continuarem idempotentes. Forma:

```
---
description: pstack per-role model choices (overrides skill defaults)
alwaysApply: true
---
# pstack model configuration. One line per role. Delete a line to fall back to the skill default.
# `inherit-parent` or `auto` as a value: the role runs on the parent chat model (omit Task `model` and spawn `generalPurpose`). Alias entries in a panel list still count toward its fan-out.
# defaults: 3
# budget: large (xhigh)
feature, refactoring: claude-opus-5-5-xhigh-fast
bug-fix: claude-opus-5-5-xhigh-fast
perf-issue: claude-opus-5-5-xhigh-fast
hillclimb: claude-opus-5-5-xhigh-fast
judgment and prose: claude-opus-5-5-xhigh-fast
arena runners: claude-fable-5-2-high, claude-opus-5-5-high-fast, gemini-4-carbon-high
arena cross-judge pool: claude-opus-5-5-high-fast, gemini-4-carbon-high
swarm workers: claude-opus-5-5-high-fast
interrogate reviewers: claude-fable-5-2-high, claude-opus-5-5-high-fast, gemini-4-carbon-high
```

### 6. Confirme

Diga ao usuário que a regra foi escrita e que ela vale para sessões novas. Rodar esta skill de novo atualiza a regra.

### 7. Ofereça uma skill de verificação (opcional)

Verifique se o projeto tem um jeito de dirigir o app real como prova (uma skill `verify-*`, ou um harness existente). Se não tiver, ofereça uma vez: "quer uma skill de verificação local do projeto, para agentes dirigirem o app como um usuário faz e provarem que as mudanças funcionam? Eu gero uma com /create-verification-skill." Se sim, invoque `/create-verification-skill` (resolve onde o pstack estiver instalado: workspace, usuário ou plugin). Se não, siga em frente sem insistir.

### 8. Ofereça uma skill de verificação formal (opcional)

Verifique se o projeto já tem uma skill de verificação formal (uma skill de projeto que roda um model checker, uma prova, ou outro verificador contra o código). Se não tiver, ofereça uma vez: "quer uma skill de verificação formal, para agentes provarem que uma propriedade-chave do seu código vale em todo caso no escopo, não só nos casos que os testes alcançam? Eu gero uma com /create-formal-verification-skill." Se sim, invoque `/create-formal-verification-skill` (resolve onde o pstack estiver instalado: workspace, usuário ou plugin). Se não, siga em frente sem insistir.
