---
name: maintain-verification-skill
description: "Passada periódica que mantém honestos a skill de verificação de um projeto e o mapa de features: leitores de código em paralelo por feature, uma sessão ao vivo dirigindo toda feature, no máximo um PR de correções provadas. Use para /maintain-verification-skill ou \"audit the verify skill\"."
disable-model-invocation: true
---

# Manter uma skill de verificação

Um mapa de features apodrece assim que o app muda. Esta skill é o laço de manutenção de uma skill gerada pelo `/create-verification-skill` (ou qualquer skill de verificação do projeto com um mapa de features). A unidade de rigor é a feature, não cada frase. Cubra todo arquivo de feature no código e exercite toda feature ao vivo, sem transformar cada item em comando de terminal.

## Resultados

Escolha um e diga qual:

- **limpo**. Toda feature teve cobertura de código e ao vivo, nada que valha entregar. Sem branch, sem PR.
- **mudou**. Um PR entrega correções provadas de doc, harness ou mapa.
- **bloqueado**. A cobertura não terminou ou uma correção provada não pôde ser entregue com segurança. Diga exatamente o que bloqueou.

## Escopo de edição

Edite só o diretório da própria skill de verificação (o SKILL.md dela, features/, e os scripts de harness que ela possui). Nunca edite código de produto durante uma execução. Um comportamento que o mapa descreve e o app não faz mais é desvio de doc (conserte o mapa) ou regressão de produto (relate, não esconda em doc).

## Passada

0. **Localize o alvo.** Ache a skill de verificação a manter. É a skill local ao projeto cujo corpo tem seções de lançamento e condução e um mapa de features (normalmente `.cursor/skills/verify-*/`). Vários candidatos → pergunte qual; nenhum → pare e aponte para `/create-verification-skill` em vez de inventar um alvo.

1. **Higiene do índice.** Leia o README do mapa de features e faça glob dos arquivos irmãos dele. Conserte entradas faltando, sobrando, duplicadas ou mortas. Leve, sem inventário gerado.

2. **Onda de código.** Um subagente só de leitura por arquivo de feature, em paralelo. Cada um explica "como esta feature voltada ao usuário funciona?" a partir do código, sinaliza provável desvio de doc com citações, e devolve uma receita concisa de verificação ao vivo. Os filhos nunca dirigem o app e nunca editam arquivos. Formato do retorno. resumo da feature / pontos de entrada no código / provável desvio ou nenhum / uma receita.

3. **Reconcilie.** Todo arquivo de feature tem um resumo. Funda receitas sobrepostas no menor número prático de estados do app. Confira por amostragem o desvio citado, não reprove afirmação limpa. Varra a agitação recente procurando superfícies voltadas ao usuário que faltam no mapa. Exija um caminho de código concreto antes de chamar uma de faltante.

4. **Passada ao vivo.** Obrigatória mesmo com código limpo. O coordenador é dono de toda a condução. Siga o modelo de lançamento da skill de verificação, uma instância de vida longa dirigida em série para servidores e UIs, ou uma sessão isolada nova por condução para CLIs de vida curta (a seção Launch da skill decide, não esta). Exercite toda feature ao menos uma vez, e sustente três invariantes na passada inteira, qualquer que seja a falha. (1) nunca dirija uma instância que você não checou com doctor desde a última vez que ela fez algo surpreendente, doctor antes da primeira condução, doctor em cada sessão nova onde sessões são a unidade, doctor de novo depois de qualquer condução que falhou, e onde o doctor não vê a falha (um estado de UI travado num processo saudável), resete para um estado conhecido ou relance em vez de torcer; (2) a evidência capturada até ali sobrevive a toda limpeza, checada no local nomeado dela, não presumida; (3) nada que uma condução iniciou sobrevive à utilidade dessa condução, resíduo de iteração falha é limpo esteja a sessão travada, encerrada ou compartilhada (numa instância compartilhada, limpe o resíduo, não a instância). Uma falha de doctor causada por desvio da skill é desvio. Conserte sob o escopo de edição e tente uma vez, reinicie o que a correção invalidou, nada além, antes de chamar a passada de `bloqueado`. Uma feature que não dá para alcançar é `verified-unreachable` só com o pré-requisito concreto (auth, entitlement, SO, estado externo) e a rota tentada; se o mapa omite esse pré-requisito, é desvio. Toda correção de harness da triagem é dirigida ao vivo de novo antes de entregar. O desmonte final vem depois da última condução, incluindo essas reprovas, para nada sobreviver à execução (a evidência fica, conforme a skill).

5. **Triagem.** Descrição do ponto de vista do usuário errada ou faltando → desvio de doc, conserte. Comportamento que funciona mas o harness não dirige → lacuna de harness, conserte; uma correção de harness segue a mesma regra de helpers da geração (scripts executáveis, invocação documentada no corpo da skill). Comportamento do app que está quebrado de fato → lacuna de produto; registre para o usuário, mantenha fora deste PR.

6. **Entregue ou pare.** Se mudou, um PR de correções provadas, e releia todo arquivo alterado antes. Se limpo ou bloqueado, nenhum PR, e relate o resultado e a cobertura com honestidade.

Mantenha notas concisas da execução (features cobertas, pré-requisitos inalcançáveis, desvio confirmado, resultado) num local de rascunho, não commite elas.
