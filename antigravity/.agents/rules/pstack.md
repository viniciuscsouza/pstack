---
trigger: always_on
description: "Contexto sempre ativo do pstack em PT-BR: paths das skills, invocação, mapeamento de ferramentas do harness e invariantes do fluxo de trabalho."
---

# pstack

Este workspace usa o pstack, em português do Brasil. As skills ficam em `.agents/skills/` e carregam sozinhas quando a `description` casa com a tarefa. O agente também pode invocar pelo nome, como `/poteto-mode` ou `/unslop`.

## Como usar

- Peça para trabalhar no estilo do poteto, ou chame `/poteto-mode`. Ele é o orquestrador: escolhe o playbook, mantém a lista de tarefas e delega para o agente `poteto-agent` em `.agents/agents/`.
- Antes do primeiro passo que nomeia uma ferramenta do Cursor, leia `docs/harness/antigravity.md` uma vez por sessão. Ele traz o mapa de ferramentas, agentes, modelos e paths, e diz como substituir Automations, Bugbot e agentes na nuvem.
- Fan-out de subagentes usa Dynamic Workflow, não um tool de Task.

## Modelos

Cada papel herda o modelo da sessão. O frontmatter de skill aqui aceita só `name` e `description`, então modelo se escolhe na sessão, nunca no arquivo da skill.

Se precisar fixar papéis, a tabela vive em `.agents/rules/pstack-models.md`, com `trigger: always_on` no frontmatter. Limite conferido em 2026-11-23 na doc oficial do Antigravity: 24.000 bytes por arquivo de regra, após expandir includes, e orçamento agregado de 20.000 tokens para regras globais e `always_on`. Se a tabela passar do limite por arquivo, quebre em dois arquivos, cada um com frontmatter `trigger: always_on`.

## Regras que valem sempre

- Nomeie a forma dos dados antes de escrever lógica.
- Verifique contra o artefato real, nunca contra um proxy ou um "compila".
- Corrija a causa raiz. Uma guarda só é correção quando o estado que ela trata é legítimo.
- Não entregue ao humano uma checagem que você poderia rodar.
