---
name: create-verification-skill
description: "Gere uma skill de verificação local ao projeto que dirige seu app do jeito que um usuário dirige, em qualquer linguagem, framework ou plataforma. Use para /create-verification-skill, \"make a control skill for this repo\", ou quando um projeto não tem jeito roteirizado de provar o comportamento de UI/CLI/serviço."
---

# Crie uma skill de verificação

Todo projeto sério precisa de um jeito roteirizado de dirigir o app real e provar comportamento: iniciar, exercitar uma funcionalidade do jeito que um usuário faria, e capturar evidência. Esta skill gera isso como uma skill local ao projeto (`.cursor/skills/verify-<app>/`) ajustada ao repositório. Você escreve a saída do gerador para o próximo agente, não para um humano. Ela vai ser lida a frio, no meio da tarefa, por um agente que nunca viu o app.

## 1. Entreviste o repositório, não o usuário

Responda isto a partir da base de código e só pergunte ao usuário o que não consegue observar:

- **Superfície:** o que um usuário de fato toca? Uma UI web, um CLI/TUI, um app desktop, uma API, um app mobile, uma biblioteca? Um repositório pode ter várias; escolha a principal e anote as demais.
- **Execução:** como o app inicia localmente? Prefira o comando de dev documentado pelo próprio repositório (scripts de pacote, Makefile, quickstart do README). Anote portas, variáveis de ambiente, dados de seed, autenticação.
- **Direção:** como um agente interage com ele programaticamente? Harnesses existentes primeiro, specs Playwright/Cypress, scripts expect, helpers de PTY, endpoints acessíveis por curl, uma porta de debug. Só depois escolha uma receita genérica: navegador/CDP para web e Electron, um harness de tmux/PTY para CLI/TUI, HTTP puro para serviços.
- **Observação:** que evidência dá para capturar? Capturas de tela, transcrições de terminal, corpos de resposta, logs, códigos de saída, estado do banco.
- **Isolamento:** duas instâncias rodam lado a lado (portas, diretórios de dados, perfis)? Se não, diga isso na skill gerada. Recusar dirigir duas vezes uma instância compartilhada é melhor que corromper a sessão do usuário.

Se o checkout não compila ou não inicia como está, conserte isso primeiro (ou relate com precisão) antes de gerar. Uma skill escrita contra uma base quebrada ensina passos errados. Quando um ativo ausente e irrelevante bloqueia a inicialização (um diretório estático que a API nunca serve, uma config de exemplo), a skill gerada pode criar ele, marcado claramente como andaime de verificação, e removê-lo na limpeza.

## 2. Gere a skill

Escreva `.cursor/skills/verify-<app>/SKILL.md` com frontmatter YAML (`name: verify-<app>` e uma `description` que nomeia o app, a superfície, e quando recorrer a ela; sem frontmatter a skill nunca registra) e estas seções, cada uma ancorada no que a entrevista de fato encontrou (nenhum placeholder sobrando):

- **Lançamento:** o comando exato que inicia o app para verificação, e como saber que está pronto (uma linha de log, um prompt). O lançamento precisa chegar a um estado utilizável a partir de um diretório de dados vazio, incluindo migrações e dados de seed. Pronto significa utilizável, não só que um processo responde. Uma checagem de saúde do banco pode passar enquanto os scripts de init ainda rodam. Inclua o teardown. Para um CLI ou TUI curto não há servidor para manter vivo. Lançar significa compilar o binário (ou instalar dependências) uma vez, depois iniciar cada execução na própria sessão de PTY ou tmux isolada.
- **Doctor:** uma checagem somente leitura que responde "esta instância merece ser dirigida?", com processo no ar, versão/build certo, porta sob nosso controle, autenticação válida. Um agente roda isso primeiro sempre que algo parece estranho.
- **Direção:** a receita do harness com seletores e comandos reais deste repositório, não exemplos. Prefira alças estáveis (rótulos ARIA, data attributes, strings de prompt, caminhos de rota) a coordenadas e ordem de tab.
- **Evidência:** o que capturar para uma prova e onde isso vai. Declare os padrões de prova. Exercite o caminho real do usuário, não setters internos ou endpoints só de teste. Capture a ação e o estado resultante, não só a tela final. Verifique efeitos colaterais (arquivos escritos, linhas inseridas, mensagens enviadas) junto com o que é visível. Mocks só onde uma fronteira de produção já isola o sistema externo. Quando o caminho seguro é um dry-run ou modo de teste, verifique o que ele de fato pula observando (arquivos, rede, refs do git) em vez de confiar no nome. Alguns dry-runs ainda tocam a rede ou abrem um navegador.
- **Limpeza:** como derrubar instâncias que a execução criou. Nunca mate por nome de processo. Mate o que você iniciou. A limpeza remove instâncias e estado de rascunho, nunca a evidência. Artefatos de prova sobrevivem ao teardown, num local que a skill nomeia.
- **Helpers:** todo script que a skill entrega é executável e a invocação dele aparece no corpo da skill. Um helper que o leitor precisa engenharia reversa não é helper.

## 3. Semeie o mapa de funcionalidades

Crie `.cursor/skills/verify-<app>/features/README.md` mais um arquivo por funcionalidade voltada ao usuário que você conseguir identificar (mire nas 3 a 5 principais para começar, a partir de rotas, comandos, menus ou docs). Siga a forma em [`references/feature-map-example/`](references/feature-map-example/), com um índice README e um arquivo por funcionalidade. Cada arquivo responde, do ponto de vista do usuário, o que a funcionalidade é, como chegar nela, como dirigi-la com o harness, e que estado final observável prova que ela funciona. Os quatro H2 são `Sub-features`, `How to get to it (user POV)`, `Driving it with <harness>` e `Gotchas`. Se os dados de seed não têm uma pré-condição que uma jornada precisa, a jornada relata uma falha e o motivo vai nos `Gotchas` dessa funcionalidade. Nunca enfraqueça a jornada para ela passar. O mapa é a fonte de verificação mantida do repositório. Uma prova que dirige um ponto de entrada conveniente é incompleta quando o mapa lista outros.

## 4. Prove a skill gerada antes de entregá-la

Rode as instruções dela de ponta a ponta uma vez: lançar, doctor, dirigir UMA funcionalidade mapeada (uma basta, o mapa existe para execuções posteriores cobrirem o resto), capturar evidência, limpar. Depois da limpeza, confirme que a evidência ainda existe no local nomeado. Uma limpeza que come a prova falha neste passo. Conserte o que falhar, e rode a limpeza gerada depois de toda iteração falha também, para tentativas quebradas não deixarem processos e portas órfãos. Uma skill gerada que nunca foi executada é rascunho, não entrega. Se você provou num substituto do alvo real (um contêiner no lugar de uma VM limpa, uma stack local no lugar de uma remota), liste todo passo de setup que o substituto fez e o alvo não fará. Esses passos ficam não verificados até uma execução no alvo real cobri-los.

## 5. Ofereça o ciclo de manutenção

Aponte o usuário para `/maintain-verification-skill` para manter o mapa honesto conforme o app muda. Sugira uma cadência só se ele pedir.