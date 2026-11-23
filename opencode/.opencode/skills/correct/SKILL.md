---
name: correct
description: "Ache os erros que agentes repetem neste repo e torne cada um impossível. Tente arquitetura primeiro, depois tipos, depois um lint cujo erro nomeia a correção, depois um teste, e escreva docs por último. Prove que cada checagem falha num erro real do passado. Repita isso a cada vez que o operador te corrigir. Use para /correct."
---

# Corrija

O operador vive corrigindo agentes neste repo pelos mesmos erros. Mude o repo para o próximo agente não conseguir cometê-los.

Assuma que todo colaborador é um agente que só vê os arquivos que abriu, copia o exemplo mais próximo, e pega o caminho mais curto que compila. Desenhe o repo para uma mudança que parece certa de um arquivo ser certa para o repo inteiro.

## Ache as classes de erro

Primeiro, leia commits recentes, reverts, comentários de revisão, arquivos de instrução de agente, e comentários que explicam contornos. Agrupe os erros em classes. Uma classe conta quando já aconteceu duas vezes.

## Corrija cada classe no nível mais alto que funciona

1. **Elimine com arquitetura.** Dê a cada pedaço de estado um dono e a cada tarefa um caminho suportado. Esconda internos para o import errado falhar. Troque listas sincronizadas na mão por uma fonte única de verdade. Delete caminhos antigos e código morto que um agente copiaria.
2. **Aplique com tipos para o estado ruim não poder ser escrito.** Se código ruim ainda compila, adicione um lint ou checagem de CI cujo erro nomeie o arquivo, tipo ou função a usar no lugar. Se o padrão já é comum, falhe só quando uma mudança adiciona mais.
3. **Teste o comportamento.** Conserte ou delete qualquer teste que ainda passa quando o comportamento que ele checa quebra, ou que falha quando só os internos mudam.
4. **Escreva docs ou regras de agente por último, só para julgamentos.** Nada falha quando um agente as ignora.

## Corrija e prove

Depois corrija agora as classes mais frequentes, um commit cada. Prove que cada checagem nova falha num erro real do passado. Rode o mesmo comando local e no CI. Exceções vão na linha infratora com um motivo, uma data de expiração, e a aprovação de um humano.

## Mantenha a tabela de regras

Por último, mantenha no arquivo de instrução de agente uma tabela que pareia cada regra com o que a aplica. Quando o operador te corrige, corrija o erro e adicione a regra. Se a regra já estava lá e nada a aplica, é reincidência, então corrija no nível mais alto na mesma mudança. Remova uma regra quando o erro dela não pode mais acontecer.

**Resposta:** cada classe com a evidência dela, o nível que você escolheu, e por que um nível mais alto não funcionou.
