### Prototype

**Você é dono da decisão de design, não do código. O protótipo é um instrumento descartável. A construção real segue Feature.**

O único playbook onde a "menor mudança" do Protocolo da Preguiça e a barra de verificação se invertem. Velocidade acima de acabamento, qualidade de código não importa, sem planejamento. O rigor está em escolher o design certo de forma barata. Proponha variações que o usuário não pediu, jogue uma abordagem fora e tente outra.

1. Delimite a decisão que o protótipo existe para tomar. Qual layout, qual interação, qual densidade, ou para uma bifurcação empírica qual comportamento, timing ou abordagem. Sem decisão não há protótipo. Roteie para Feature.
2. Junte referências quando o espaço de design está aberto. Busque arte prévia, resuma um moodboard de temas, paletas e layouts, deixe o usuário escolher direções antes de construir. Pule quando a direção está definida.
3. Construa descartável num diretório scratch isolado, separado do código de produção. Para uma decisão visual, HTML/CSS/JS puro ou a stack mais leve que renderiza a ideia, dependências de CDN, um dev server com hot reload. Para uma decisão de comportamento ou timing, o menor script que exercita a pergunta. Sem framework de produção, sem testes, sem abstrações.
4. Ao comparar alternativas, construa-as atrás de um único alternador (botões ou um toque de tecla), cada variante rotulada. Esta é a skill de princípio **exhaust-the-design-space** feita barata.
5. Verifique na superfície correspondente. Para uma decisão visual, tire screenshot de cada variante pela skill de controle e dirija a interação. Para uma decisão de comportamento ou timing, observe a coisa que você está decidindo registrando o timing, imprimindo a saída ou olhando o render. A observação é o teste aqui, não uma asserção.
6. Apresente alternativas, tradeoffs e uma recomendação. A saída é a decisão mais o artefato descartável, não código entregável. Entregue a direção escolhida para **Feature** (ou `architect` para a forma) para a construção real.

**Resposta:** as variantes exploradas, a evidência (screenshots para uma decisão visual, a saída ou o timing observado para uma comportamental), tradeoffs, a sua recomendação, e o caminho do scratch. Diga com clareza que o protótipo é descartável.
