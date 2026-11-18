### Multi-phase or multi-PR plan

**Você é dono do plano, não do código. O plano é uma lista de checagem que um dono roda caixa por caixa e o operador audita a partir da evidência.** O plano é a entrega. Não implemente.

1. Quando a mudança é um ou dois arquivos com abordagem óbvia, pule o plano. Diga isso e pare.
2. Resolva questões abertas por protótipo antes de escrever, conforme `playbooks/prototype.md`. Pergunte ao operador só sobre uma decisão de produto ou preferência que nenhuma execução resolve.
3. Escreva o arquivo do plano. Abra com o que muda, os ids de PR em ordem de aterrissagem, e o grafo de dependência, que diz quais PRs são independentes e ramificam do trunk, quais esperam quais, e os arquivos que cada um pode tocar. Depois uma seção por PR. Um PR é uma mudança com evidência própria (a skill de princípio **sequence-verifiable-units**). Cada seção tem caixas de checagem para o de que ela depende, os arquivos que edita, a única mudança que constrói, o resultado que você vê, e a verificação dela.
4. Testes sozinhos não são verificação suficiente. Um PR só está verificado quando as caixas de unidade, ao vivo e perf estão todas marcadas. Cada caixa ao vivo conduz a superfície real com um cenário concreto e um predicado de aprovação. A caixa de perf nomeia a métrica, a sonda, a linha de base do trunk medida primeiro, e o número que reprova. Um PR que muda uma interação espera a revisão do operador antes do merge.
5. Devolva o caminho do plano e pare. A execução começa no go explícito do operador.

**Resposta:** o caminho do plano, os ids de PR com as dependências e o conjunto com portão de revisão, e o que fica sem prova.
