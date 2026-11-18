---
name: principle-boundary-discipline
description: "Aplique ao ligar validação, tratamento de erro ou adaptadores de framework. Concentre guardas nas fronteiras do sistema (CLI, config, rede, APIs externas); confie nos tipos internos e mantenha a lógica de negócio em funções puras."
disable-model-invocation: true
---

# Disciplina de Fronteira

Valide, estreite tipos e trate erros onde o dado cruza uma fronteira do sistema (args de CLI, arquivos de config, APIs externas, rede). Dentro, confie nos tipos. Sem revalidação e sem checagens de nil redundantes. Faça parse de dado cru em tipos de domínio na fronteira, e mantenha tipos de transporte e de framework fora da superfície pública. A lógica de negócio vive em funções puras sem dependência de framework. A casca que chama elas é fina.

Dois testes. Este dado está cruzando uma fronteira agora? Se não, a validação é redundante. Isto pode ser uma função pura que a casca só chama? Se sim, extraia.
