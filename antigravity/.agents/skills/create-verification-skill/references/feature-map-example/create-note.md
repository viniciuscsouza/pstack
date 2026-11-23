# Criar uma nota

Criar nota deixa o usuário salvar uma nota com título pelo navegador ou pela CLI, cancelar um rascunho inacabado, e confirmar a nota salva a partir de uma segunda visão voltada ao usuário.

## Subfuncionalidades

- `create-open` abre um editor em branco a partir de cada ponto de entrada do navegador.
- `create-save` persiste um título e um corpo.
- `create-cancel` descarta um rascunho inacabado do navegador.
- `create-cli` cria a mesma forma de nota a partir do terminal.

## Como chegar nela (ponto de vista do usuário)

- Escolha o botão `New note` na barra de ferramentas do navegador.
- Pressione `n` no navegador com o foco fora de um campo editável.
- Rode `notes create --title <title> --body <body>` num terminal.

## Conduzindo com control-notes

Pré-condições:

- O Notes está saudável em `http://127.0.0.1:4173`.
- Nenhuma nota tem o título `Release checklist`.
- `control-notes doctor` reporta a URL esperada e o diretório de dados descartável.

- **Abrir o editor.** Escolha `New note`. Rode `control-notes browser click --role button --name "New note"`. Um formulário chamado `Note editor` aparece com o foco na caixa de texto `Title`.
- **Inserir conteúdo.** Digite o título e o corpo. Rode `control-notes browser fill --role textbox --name "Title" --value "Release checklist"` e `control-notes browser fill --role textbox --name "Body" --value "Tag and publish"`. O botão `Save note` fica habilitado.
- **Salvar a nota.** Escolha `Save note`. Rode `control-notes browser click --role button --name "Save note"`. Um status chamado `Note saved` aparece e o cabeçalho mostra `Release checklist`.
- **Confirmar a persistência.** Volte à lista de notas e reabra a nota. Rode `control-notes browser click --role link --name "All notes"` e `control-notes browser click --role link --name "Release checklist"`. O editor mostra os dois valores salvos.
- **Cancelar o rascunho.** Abra uma nota nova, digite `Discard me`, e escolha `Cancel`. Rode `control-notes browser click --role button --name "New note"`, `control-notes browser fill --role textbox --name "Title" --value "Discard me"`, e `control-notes browser click --role button --name "Cancel"`. A lista de notas volta e não tem link `Discard me`.
- **Entrada pela CLI.** Crie uma segunda nota. Rode `control-notes cli -- notes create --title "CLI note" --body "Created from terminal" --format json`. O código de saída `0` e a stdout contêm o ID e o título da nota nova.
- **Prova.** Reabra as duas notas salvas em `All notes`. Rode `control-notes browser snapshot --aria --path artifacts/create-note/list.aria.txt` e `control-notes browser screenshot --path artifacts/create-note/list.png`. Os artefatos mostram `Release checklist` e `CLI note`.

## Armadilhas

- Pressionar `n` com uma caixa de texto em foco digita o caractere em vez de abrir um editor novo.
- Títulos são aparados ao salvar. Afirme o título renderizado, não o valor do input do rascunho.
- Um status de salvamento sozinho é prova insuficiente. Reabra a nota pela lista.
- Remova `Release checklist` e `CLI note` na limpeza de fixture, mas retenha os artefatos de prova delas.
