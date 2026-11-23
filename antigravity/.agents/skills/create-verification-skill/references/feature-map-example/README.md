# Mapa de verificação do Notes

Este diretório é a fonte mantida para verificar o comportamento do Notes voltado ao usuário. Leia o índice antes de dirigir o app, depois use o arquivo de funcionalidade correspondente como receita.

## Pré-condições de base

- Inicie o Notes em `http://127.0.0.1:4173` com um diretório de dados descartável.
- Defina `NOTES_DATA_DIR=/tmp/notes-verify-$RUN_ID` para que execuções concorrentes não compartilhem estado.
- Semeie notas com os títulos `Quarterly plan` e `Grocery list`.
- Ponha `control-notes` e a CLI `notes` no `PATH`.
- Rode `control-notes doctor` e exija a URL, o diretório de dados e a revisão de build esperados.
- Nunca dirija uma instância que não foi iniciada por esta execução de verificação.

## Convenções de condução

- Comece toda receita a partir do estado de base, a menos que as pré-condições dela digam outra coisa.
- Prefira papéis ARIA e nomes acessíveis a seletores CSS ou posição no DOM.
- Trate todo comando como literal. Mantenha nomes entre aspas e flags sem alteração.
- Rode ações de navegador por `control-notes browser`.
- Rode ações de terminal por `control-notes cli -- <command>`.
- Restaure os dados semeados depois de uma mutação. Não remova artefatos de prova durante a limpeza.

## Prova e relato de pulos

- Capture a ação do usuário e o estado resultante, não só a tela final.
- Prova de UI inclui um snapshot ARIA e um screenshot com a identidade do app visível.
- Prova de CLI inclui o comando, stdout, stderr e o código de saída.
- Prova de mutação inclui uma segunda visão somente leitura do valor armazenado.
- Registre o ID da funcionalidade e o ponto de entrada usados em cada artefato.
- Relate um caminho inalcançável com o comando tentado e a pré-condição não atendida.
- Não relate um ponto de entrada pulado como verificado por outro caminho.

## Contrato de entrada de funcionalidade

Cada arquivo de funcionalidade começa com um título H1 e um parágrafo descrevendo o comportamento visível ao usuário. Depois usa exatamente quatro seções H2 nesta ordem.

1. `Sub-features` lista IDs curtos com uma linha para cada comportamento.
2. `How to get to it (user POV)` lista todo ponto de entrada do usuário.
3. `Driving it with <harness>` começa com `Preconditions:` e usa itens rotulados que pareiam cada ação do usuário com um comando exato e um resultado observável.
4. `Gotchas` lista armadilhas que podem desperdiçar ou invalidar uma execução de verificação.

Mantenha detalhes de implementação fora do mapa. Nomeie só caminhos de usuário, identificadores estáveis, estado necessário, comandos e prova observável.

## Funcionalidades

- [Criar uma nota](./create-note.md) cobre criação no navegador e na CLI, cancelamento, persistência e limpeza.
- [Buscar notas](./search.md) cobre busca por barra de ferramentas, teclado e CLI, com estados de correspondência, vazio e limpo.
