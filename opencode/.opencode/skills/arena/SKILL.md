---
name: arena
description: "Crie N candidatos em paralelo na mesma tarefa, escolha uma base, enxerte nela as partes mais fortes dos perdedores. Use para /arena, 'jogue isso na arena', 'arena nisto', ou quando uma única tentativa num artefato não trivial travaria a forma errada."
---

# Arena

Abra uma lista de tarefas com uma entrada por fase (Enquadrar, Abrir em leque, Julgamento cruzado, Escolher, Enxertar, Verificar) antes de disparar qualquer coisa.

## Fase A: Enquadrar

Todo candidato recebe o mesmo prompt, então o prompt é o contrato.

1. Declare o artefato que cada candidato está produzindo.
2. Derive a rubrica, 3 a 6 critérios concretos e avaliáveis para o que é sucesso nesta tarefa. Ela é a ferramenta de quem escolhe na Fase D. Os candidatos só veem a tarefa.
3. Escolha os executores na linha `arena runners` de `~/.cursor/rules/pstack-models.mdc`, por padrão `claude-fable-5-2-high`, `claude-opus-5-5-high-fast` e `gemini-4-carbon-high`. Uma entrada `auto` ou `inherit-parent` é o modelo pai, então omita `model` e crie um `generalPurpose`. Se a ferramenta Task rejeitar um slug, use o slug válido mais próximo da mesma família ou descarte o assento, mantenha pelo menos dois executores, e diga isso. As famílias vão por prefixo, e a correspondência mais longa vence: `claude-fable-*`, `claude-*` e `gemini-*`, depois de descartar um `cursor-` inicial. A família de qualquer outro slug é o texto dele antes do primeiro hífen. Crie mais para várias direções de design, e repita um modelo quando o trabalho é limitado por geração, não sensível a julgamento. Escolha o nível de raciocínio de cada subagente conforme a seção Subagentes da skill **poteto-mode**.
4. Dê a cada candidato a própria localização de saída (um worktree git quando possível, senão `/tmp/arena-<slug>/candidate-<n>/`), conforme a skill de princípio **separate-before-serializing-shared-state**.

## Fase B: Abrir em leque

Crie todos os N subagentes numa única mensagem com `run_in_background: true`, cada um com a tarefa, o caminho do embasamento compartilhado, o próprio caminho de saída, e instruções para produzir o artefato e uma justificativa curta nomeando as alternativas que considerou e o que rejeitou.

Se um candidato não produz saída, siga com N-1 e registre a desistência.

## Fase C: Julgamento cruzado

Depois que todos os candidatos da Fase B terminarem, crie um juiz somente leitura a partir da linha `arena cross-judge pool` de `~/.cursor/rules/pstack-models.mdc`, por padrão `claude-opus-5-5-high-fast`, `gemini-4-carbon-high`. O juiz nunca vem do fornecedor do pai, e Fable conta como Claude nessa checagem, então use a primeira entrada de outro fornecedor. Uma entrada `auto` ou `inherit-parent` é o modelo pai. Se você não sabe o modelo do pai, use o primeiro slug real e diga que a checagem de fornecedor não rodou. Se a ferramenta Task rejeitar um slug, use o slug válido mais próximo da mesma família, depois a próxima entrada desse tipo, ou pule o juiz, e diga isso. Enquanto você lê na Fase D, o juiz pontua cada candidato, por rótulo de caminho, em cada critério da rubrica e recomenda uma base.

## Fase D: Escolher uma base

Leia todo candidato de ponta a ponta antes de escolher. Pontue cada um contra a rubrica critério por critério, não por sensação holística, e compare com o juiz cruzado quando a Fase C rodou um. Concordância na base confirma a escolha. Discordância significa que um dos dois tem viés ou a rubrica estava ambígua. Leia as duas justificativas antes de decidir.

Escolha a base que um mantenedor futuro consegue estender com mais facilidade sem quebrar invariantes. Quando duas parecem empatadas, prefira a fronteira mais limpa ou a API menor, conforme o Protocolo da Preguiça.

## Fase E: Enxertar

Percorra cada candidato perdedor mais uma vez atrás do que vale portar para a base, normalmente uma ou duas coisas por candidato, não a maior parte dele. Incorpore cada enxerto à mão, conforme a skill de princípio **redesign-from-first-principles**, para o resultado continuar coerente sob um único modelo mental. Não cole de forma mecânica.

Se os candidatos convergem numa forma só, isso é concordância forte. Registre e entregue a forma consensual. Nenhum enxerto é necessário. Se eles divergem muito, a Fase A ficou subespecificada. Reenquadre e rode de novo em vez de tirar a média da divergência.

## Fase F: Verificar

Verifique o artefato sintetizado como qualquer outra saída, conforme a skill de princípio **prove-it-works**. Se a verificação encontra um problema que a arena deixou passar, ou a Fase A estava errada (reenquadre e rode de novo) ou um candidato pegou o problema e você perdeu o enxerto (volte à Fase E). Não esconda o problema.

## Saídas

Um artefato sintetizado, com uma nota curta de síntese ao lado nomeando a base e por quê, o veredito do juiz cruzado ou que a Fase C pulou o juiz, cada enxerto com o candidato de origem, o que foi rejeitado e por quê, qualquer convergência ou desistência, e o resultado da verificação.
