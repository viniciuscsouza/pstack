# pstack

Este repositório usa o pstack, em português do Brasil. As skills ficam em `.github/skills/` e o Copilot carrega cada uma quando a `description` casa com a tarefa.

## Como usar

- Peça para trabalhar no estilo do poteto, ou invoque o prompt `/poteto-mode`. Ele é o orquestrador: escolhe o playbook, mantém a lista de tarefas e delega para o agente `poteto-agent`.
- Uma skill isolada se chama lendo `.github/skills/<nome>/SKILL.md` e seguindo. Quando o usuário digitar um comando, use o prompt file correspondente em `.github/prompts/`.
- Antes do primeiro passo que nomeia uma ferramenta do Cursor, leia `docs/harness/copilot.md` uma vez por sessão. Ele traz o mapa de ferramentas, agentes, modelos e paths, e mostra que Bugbot e agentes na nuvem existem nativamente aqui como Copilot code review e Copilot cloud agent.

## Modelos

Cada agente herda o modelo da sessão por padrão. Para fixar, use o campo `model` do agente personalizado com um identificador que o ambiente aceita, mais `reasoningEffort` quando houver. Não copie slug do Cursor.

## Agentes disponíveis

- `poteto-agent` para escrita de código e tarefas auxiliares.
- `comment-sicko` para caçar comentário inútil antes do commit.
- Embutidos do Copilot: `explore` para leitura, `code-review` para revisão de diff, `general-purpose` para trabalho isolado, `security-review` para revisão de segurança.

## Regras que valem sempre

- Nomeie a forma dos dados antes de escrever lógica.
- Verifique contra o artefato real, nunca contra um proxy ou um "compila".
- Corrija a causa raiz. Uma guarda só é correção quando o estado que ela trata é legítimo.
- Não entregue ao humano uma checagem que você poderia rodar.
