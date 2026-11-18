# Notas de busca

A busca deixa um usuário encontrar notas por título ou texto do corpo, inspecionar uma nota que casa, e distinguir nenhum resultado de uma busca indisponível.

## Sub-features

- `search-open` abre a busca a partir de cada ponto de entrada do navegador suportado.
- `search-match` devolve resultados por título e por corpo sem mudar dados da nota.
- `search-open-result` abre um resultado no editor de notas.
- `search-empty` mostra um estado vazio completo para uma consulta sem resultados.
- `search-clear` limpa a consulta e restaura a visão de notas recentes.
- `search-cli` devolve as mesmas notas encontradas pelo terminal.

## Como chegar nela (ponto de vista do usuário)

- Escolha o botão `Search` na barra de ferramentas do navegador.
- Aperte `/` no navegador com o foco fora de um campo editável.
- Rode `notes search <query>` num terminal.

## Dirigindo com control-notes

Pré-condições:

- O Notes está saudável em `http://127.0.0.1:4173`.
- O diretório de dados descartável contém `Quarterly plan` com o texto de corpo `Draft budget`.
- O `control-notes doctor` reporta a URL e o diretório de dados esperados.

- **Entrada pela barra.** Escolha o botão `Search`. Rode `control-notes browser click --role button --name "Search"`. Um diálogo chamado `Search notes` aparece com o foco no searchbox dele.
- **Entrada pelo teclado.** Feche o diálogo, dê foco à página, e aperte `/`. Rode `control-notes browser press --key "/"`. O mesmo diálogo aparece e a página não insere uma barra.
- **Casa por título.** Digite `quarterly`. Rode `control-notes browser fill --role searchbox --name "Search notes" --value "quarterly"`. A lista `Search results` contém `Quarterly plan` e não contém `Grocery list`.
- **Casa por corpo.** Troque a consulta por `budget`. Rode `control-notes browser fill --role searchbox --name "Search notes" --value "budget"`. O resultado `Quarterly plan` continua visível com um trecho do corpo.
- **Abrir resultado.** Escolha `Quarterly plan`. Rode `control-notes browser click --role link --name "Quarterly plan"`. O diálogo fecha e o título do editor lê `Quarterly plan`.
- **Estado vazio.** Reabra a busca e digite `volcano`. Rode `control-notes browser fill --role searchbox --name "Search notes" --value "volcano"`. Um status chamado `No matching notes` aparece quando a busca termina.
- **Limpar consulta.** Escolha `Clear search`. Rode `control-notes browser click --role button --name "Clear search"`. O searchbox fica vazio e a região `Recent notes` substitui a lista de resultados.
- **Casa pela CLI.** Busque pelo terminal. Rode `control-notes cli -- notes search "quarterly" --format json`. O código de saída `0` e o stdout contêm um objeto cujo título é `Quarterly plan`.
- **Erra pela CLI.** Busque por um valor ausente. Rode `control-notes cli -- notes search "volcano" --format json`. O código de saída `0` e o stdout são `[]`.
- **Prova.** Capture o estado com resultados. Rode `control-notes browser snapshot --aria --path artifacts/search/results.aria.txt` e `control-notes browser screenshot --path artifacts/search/results.png`. Os dois artefatos identificam o Notes, a consulta e `Quarterly plan`.

## Armadilhas

- Apertar `/` com o editor ou o searchbox em foco insere texto em vez de abrir a busca.
- Os resultados atualizam depois de um debounce curto. Espere a lista de resultados ou o status vazio, não um sleep fixo.
- Notas arquivadas ficam de fora a menos que o usuário ligue `Include archived`.
- A CLI usa saída legível por humanos por padrão. Use `--format json` para asserções estáveis.
- Abrir um resultado muda o estado do navegador. Reabra a busca antes de provar outra consulta.
