---
name: principle-test-behavior-not-implementation
description: "Aplique quando você escreve, muda ou mantém um teste. Afirme o que os usuários do código podem observar, para o teste falhar quando esse comportamento quebra e continuar passando quando só o interno muda."
---

# Teste Comportamento, Não Implementação

Um teste é uma afirmação sobre o que os usuários do código conseguem observar pela interface dele. Valores de retorno, erros, estado que eles conseguem ler de volta, e efeitos em sistemas externos. Helpers privados, ordem interna de chamadas e dados intermediários são implementação.

Julgue cada teste contra duas edições imaginadas.

- **Quebre o comportamento** (um valor errado, um efeito pulado ou dobrado). Se o teste ainda passa, ele não prova nada. As causas comuns são uma asserção frouxa, um valor esperado calculado pelo próprio código sob teste, e uma asserção sobre dados que o teste construiu.
- **Mude os internos** e mantenha todo resultado observável. Se o teste falha, ele está preso à implementação.

Mantenha um teste que pega a primeira edição e sobrevive à segunda. Reescreva ou apague o resto. Dirija o código pela interface pública dele com uma entrada concreta, e derive o valor esperado do requisito. O que um serviço externo recebe é comportamento. Uma chamada a um helper interno não é. Pareie "isto nunca acontece" com um caso onde acontece. Afirme o tipo de erro no qual os chamadores ramificam, não a mensagem.
