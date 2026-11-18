# Formule o prompt

Um prompt declara a intenção e a checagem de pronto. O playbook fornece os passos, então poucas frases simples vencem uma spec.

## Inclua

- O objetivo. Diga o que está errado ou o que o usuário quer.
- A checagem de pronto. Ela pode passar ou falhar. "Make it better" e uma duração não são checagem.
- A prova a mostrar. Peça a saída real do comando, um vídeo do fluxo, o valor armazenado, ou um número de antes e depois.
- O que o usuário já sabe. Um sintoma, um passo de repro, um log ou um link poupa uma busca ao agente.
- As restrições reais. "repro first", "don't change any code yet", "zero behavior change" e "let me review before proceeding" mudam cada uma o que o agente faz.

## Deixe de fora

- O como. Diga o que alcançar, e deixe espaço para o agente achar um caminho melhor.
- Uma lista de skills ou passos. Uma ordem escrita à mão descarta ou reordena passos que o playbook mantém. Nomeie uma skill só para sobrepor uma escolha.
- A teoria do usuário sobre a causa, até o agente reformular o problema. Um palpite declarado estreita a busca.

## Carregue o contexto primeiro

- Num relato confuso, peça ao agente para reformular o problema de base com as próprias palavras, em linguagem simples, antes de fazer qualquer outra coisa. Uma leitura errada aparece antes de existir código.
- Num chat novo, `/recall` o trabalho anterior sobre o tópico. Chats antigos guardam contexto que o agente novo não tem.
- Antes de uma mudança em código desconhecido, peça `/how` para a mecânica e `/why` para as razões. Um agente sem modelo rastreado corrige o sintoma no primeiro ponto plausível.
- Peça `/teach` para defender uma escolha, como em "convince me it fixes the cause and not the symptom". Um argumento é mais fácil de checar que um resumo.

## Design antes do plano

- Nunca aceite o primeiro design. Peça protótipos de algumas opções, com screenshots ou vídeos para UI, e escolha pela evidência.
- Deixe os protótipos responderem as perguntas abertas. Não revise um plano abstrato de forma adversarial, porque revisores inventam riscos que nunca acontecem.
- Para um pacote ou API compartilhada, peça o README ou um tutorial primeiro, depois volte para o código. O doc vira o alvo contra o qual o agente se checa.
- Peça o plano só depois do design fechado. Cada passo do plano termina numa checagem.

## Continue curto

- "do it", "continue" e "keep going until done" são prompts inteiros quando o chat já tem a tarefa.
- Comece com "new task" quando o assunto muda. Senão o modo trata a mensagem como o próximo passo.

## Antes de sair

- Diga "im going to bed" ou "im stepping away" para o agente parar de perguntar.
- Escreva o pronto como checagens que toda iteração pode rodar, e dê esse predicado ao `/loop`.
- Peça um worktree novo a partir de uma base nomeada.
- Responda antes o que faria o agente parar, como "don't ask me before committing".
- Peça um log de decisões para auditar depois.
- Dê uma saída. "if you're truly stuck after a few hours, stop and write up why".

## Direcione em uma linha

- Reformule o objetivo. "i said the goal is to repro. i did not ask for a fix yet."
- Nomeie o princípio. "apply prove it works. show me the real output, not the build log."
- Um nome de princípio funciona porque o agente já leu a regra. A resposta dele nomeia a decisão que a regra mudou.
