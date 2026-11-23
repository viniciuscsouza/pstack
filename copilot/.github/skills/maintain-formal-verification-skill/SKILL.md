---
name: maintain-formal-verification-skill
description: "Realinhar a verificação formal de um projeto com o código depois que o código muda. Encontre os commits que tocaram código verificado, atualize o modelo, harness ou propriedades para descrever o código como ele está agora, rode de novo, e diga se cada falha é deriva na verificação ou bug no código. Use para /maintain-formal-verification-skill, \"is the model still true to the code\", ou depois de merges e refatorações que tocaram código verificado sem revisá-lo."
---

# Manter uma skill de verificação formal

Uma checagem formal prova uma propriedade daquilo que descreve. Quando o código muda e a verificação não, a checagem pode continuar passando enquanto descreve código que não existe mais. Essa passagem é o caso perigoso, porque parece uma prova. Esta skill é o ciclo de manutenção de uma verify skill que o `/create-formal-verification-skill` gerou, ou de qualquer skill local do projeto que roda um model checker, um prover, ou um verificador em nível de código contra código nomeado. A skill local do projeto trata a mudança que um agente faz enquanto usa essa skill. Esta skill trata as mudanças que pularam essa etapa. Merges, edições feitas sem a skill, e deriva que se acumula ao longo de muitos commits.

## Resultados

Dê a cada checagem um resultado, e diga qual.

- **em sincronia**. Nenhuma mudança desde a última sincronização altera o que a checagem descreve, e a checagem passa.
- **atualizada**. O código mudou, a verificação agora descreve o novo código, e a checagem passa.
- **bug**. A verificação descreve o código atual, e o código quebra a propriedade. Isto é um achado sobre o código. Não é deriva.
- **bloqueada**. A checagem não pôde ser realinhada ou não pôde rodar. Diga exatamente por quê.

Uma checagem pode ser atualizada e bug ao mesmo tempo. A atualização faz a verificação descrever o código de novo, e então a propriedade falha nele.

## Escopo de edição

Edite só a verificação. Modelos, harnesses, arquivos de propriedade, e a própria verify skill. Nunca edite código de produto durante uma rodada, a menos que o usuário peça. Quando uma propriedade falha no código atual, não enfraqueça a propriedade nem o modelo para fazê-la passar. Relate a falha.

## Ciclo

1. **Encontre as checagens.** Uma verify skill não tem nome fixo, então encontre-a pelo que ela faz. Uma skill de projeto que roda um model checker, um prover, ou outro verificador contra código nomeado. Leia cada uma que encontrar, ou a que o usuário nomeou. Para cada checagem, colete a propriedade, o código que ela cobre, os limites e suposições, o comando que a roda, e o commit onde ela foi confirmada pela última vez como descrição do código. Se não existe verify skill, pare e aponte para `/create-formal-verification-skill`. Se a skill não registra um commit de sincronização, use o último commit que mudou os arquivos de verificação, e diga que fez isso.

2. **Rode como está.** Rode cada checagem uma vez antes de mudar qualquer coisa, e registre o resultado. Uma passagem aqui não diz que o código está correto. Diz que a descrição antiga ainda vale. Uma falha aqui pode ser deriva ou bug. Não decida qual até o passo 5.

3. **Leia toda mudança no código coberto.** Faça diff dos arquivos cobertos do commit de sincronização até o commit atual, e leia cada mensagem de commit. Procure também código que se moveu. Uma função coberta que é renomeada, dividida, ou movida para outro arquivo continua sendo código coberto. Procure também código novo que toca o estado que uma checagem protege, como uma função nova que escreve os mesmos campos. Para cada mudança, decida qual destes casos é:
   - **Sem efeito.** A mudança não altera nada que a verificação descreve, como formatação, comentários, ou caminhos de código que a verificação não cobre. Verifique isso contra o modelo ou harness, não contra a mensagem de commit.
   - **A verificação não descreve mais o código.** Um passo que era uma ação indivisível agora são dois, um ramo foi adicionado ou removido, uma checagem se moveu, um caminho novo alcança o estado protegido, ou um valor agora tem outro intervalo. O modelo ou harness precisa mudar.
   - **A promessa mudou de propósito.** A mudança altera o que o código promete, e a evidência na mudança diz que isso é intencional. O comentário de doc, os testes, o changelog, ou a mensagem de commit. A propriedade precisa mudar, e você precisa citar essa evidência.
   - **A promessa pode estar quebrada.** O código mudou, e nenhuma evidência diz que a promessa mudou. Trate uma falha que seguir como candidata a bug.

4. **Atualize a partir do código, não do resultado.** Mude o modelo ou harness para que cada parte alterada volte a apontar para código que um revisor consegue abrir, e mantenha um passo indivisível no código como um passo na verificação. Escreva a atualização a partir do código antes de rodar de novo, e não a molde pelo que faz a checagem passar. Mude uma propriedade só para uma promessa que mudou de propósito. Anote o texto antigo, o texto novo, e a evidência. Mantenha os limites a menos que a mudança os torne errados. Se você precisar reduzir um limite, diga isso no relatório.

5. **Rode de novo e julgue cada resultado.**
   - **Passa.** A checagem agora pode passar sobre uma descrição que perdeu a capacidade de falhar. Quebre o código novo do jeito que a propriedade protege, como remover uma checagem nova ou reordenar um passo novo, e confirme que a checagem pega isso. Depois desfaça a quebra.
   - **Falha.** Reproduza o contraexemplo contra o código real, passo a passo. Se um passo não pode acontecer no código, a verificação está errada. Corrija a verificação, e diga o que estava errado. Se todo passo pode acontecer, é um bug. Nomeie o commit que o introduziu, conte o contraexemplo como uma sequência de passos nas palavras do próprio código, e declare o dano em uma frase. Se um teste barato ou uma reprodução contra o código real mostra o bug, acrescente ao relatório como evidência. Não faça commit disso no código de produto.

6. **Atualize a verify skill.** Atualize o código que cada checagem cobre, os comandos, os limites, as suposições, e o commit de sincronização. Defina o commit de sincronização como o commit que você checou. Não mova o commit de sincronização de uma checagem bloqueada, e diga isso no relatório. Depois siga a verify skill uma vez como o próximo agente faria, para saber que os passos dela ainda rodam.

## Relatório

Diga ao usuário, para cada checagem:

- o resultado, e os commits que você leu;
- o que mudou na verificação e por quê, com um motivo curto para cada mudança. O código mudou de forma, ou a promessa mudou de propósito (com a evidência);
- o resultado da nova rodada, e a quebra que você usou para mostrar que a checagem atualizada ainda pode falhar;
- cada bug, em palavras simples. O contraexemplo como passos nas palavras do código, o dano, e o commit que o introduziu;
- o que a verificação ainda não cobre, incluindo código novo que ela deveria cobrir e não cobre.

Diga abertamente quais falhas eram deriva na verificação e quais eram bugs no código. Para uma checagem que passava antes de você atualizá-la e falha depois, diga que a passagem antiga era sobre uma descrição defasada.

Ponha as atualizações de verificação em uma mudança própria, separada de qualquer correção de produto.
