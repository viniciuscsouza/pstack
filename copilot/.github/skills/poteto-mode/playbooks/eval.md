### Eval

**Você é dono do design do experimento. Planeje, cegue, rode, sintetize.**

**Inegociáveis para cegar:**

- Nada de `eval`, `test`, `judge`, `experiment`, `rubric`, `score`, `compare`, `benchmark`, `candidate` ou `arena` em qualquer diretório, arquivo, ou prompt que o candidato vê.
- O prompt do candidato parece um pedido orgânico de usuário. Declare o objetivo, não a meta.
- Sem pistas que eliciem a cadeia. Não peça ao candidato para listar quais skills, princípios, ou arquivos ele aplicou. Peça notas de design em geral e avalie o seguimento da cadeia pela forma do código, não pelo autorrelato.
- Sanitize nomes de diretório e de slug. Use nomes com cara de projeto que um usuário escolheria.
- Não diga ao candidato que existem outros candidatos.
- O juiz pode saber que está julgando, mas vê saídas só por rótulo sanitizado, nunca por nome de modelo.
- Comparar duas variantes. Um juiz pontua os dois conjuntos numa única passagem e numa única escala, cego a qual conjunto cada um veio.

**Passos:**

1. **Enquadre.** Declare qual variante está sob teste e que comportamento conta como sucesso. Escreva a rubrica (3 a 6 critérios concretos) só para o juiz. Segure-a dos candidatos.
2. **Monte ambientes sanitizados.** Diretório de trabalho por candidato com a variante no lugar. Plante qualquer contexto que uma tarefa orgânica teria. Um esqueleto de projeto, as skills que o candidato leria naturalmente.
3. **Escreva um prompt orgânico.** O que um usuário digitaria. Sem vazamento do que está sendo medido.
4. **Crie N candidatos em paralelo** em modelos diferentes conforme a Fase B da skill **arena**. Cada um trabalha no próprio diretório sanitizado. O mesmo prompt para cada. Escolha o nível de raciocínio de cada subagente para o trabalho, conforme a seção Subagentes. Um agente que só despacha, consulta, ou empacota resultados roda em `claude-opus-5-5-medium-fast`.
5. **Crie um juiz cego** numa família de modelo diferente conforme a Fase C da skill **arena**. O juiz vê saídas por rótulo sanitizado e a rubrica, nunca um nome de modelo.
6. **Verifique a cadeia por transcrições, não por autorrelato.** Leia a transcrição local de cada candidato no diretório `agent-transcripts/` do workspace ativo (o system prompt nomeia esse caminho). Não faça glob por `~/.cursor/projects/*/`. Isso cruza fronteiras de workspace e lê chats privados de projetos não relacionados. Olhe quais arquivos cada candidato realmente abriu. Avalie o seguimento da cadeia pelos arquivos que ele de fato leu mais a forma do código, nunca pelas alegações do próprio candidato.
7. **Leia toda saída de candidato você mesmo** do começo ao fim. Compare com o veredito do juiz. Discordância significa que um modelo está enviesado ou a rubrica é ambígua. Sintetize.

**Resposta:** variante sob teste, rubrica, notas por candidato, veredito do juiz, sua síntese, e uma recomendação sobre promover a variante ou não.
