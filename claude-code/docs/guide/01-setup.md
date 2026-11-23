# Configure o pstack

Nesta página você instala o plugin, escolhe quais modelos o pstack usa e roda sua primeira tarefa. A configuração é um comando mais uma conversa curta.

## Instale o plugin

Num chat do Cursor, rode:

```text
/add-plugin pstack
```

O Cursor confirma que o plugin está instalado.

No Claude Code, instale este pacote com `/plugin marketplace add viniciuscsouza/pstack`, depois `/plugin install pstack-pt-br@pstack-pt-br`. O Claude Code prefixa comandos e agentes de plugin, então cada comando deste guia leva o prefixo `pstack-pt-br:` lá, como `/pstack-pt-br:setup-pstack`, que escreve a regra de modelo em `~/.claude/rules/pstack-models.md`. O [adaptador do Claude Code](../harness/claude-code.md) cobre as demais diferenças. O identificador `pstack@cursor-plugins` é do plugin de origem, do Cursor, e não se aplica a este pacote.

## Escolha seus modelos

Rode:

```text
/setup-pstack
```

O [`/setup-pstack`](../../skills/setup-pstack/SKILL.md) detecta os modelos aos quais você tem acesso, pede um orçamento de raciocínio, mostra cada papel (delegados de código, julgamento, os painéis de revisão) e pergunta o que você quer. Responda as perguntas. Ele escreve `~/.claude/rules/pstack-models.md`, uma regra pequena que toda skill do pstack lê.

Os padrões são os mesmos do orçamento `large`. Código, prosa e julgamento rodam no Opus 5.5 com raciocínio `xhigh`, e os painéis de revisão e os workers de swarm rodam em `high`. Nenhum papel roda acima de `xhigh`. Fora de um painel de revisão, o Fable 5.2 roda só quando você o nomeia para uma tarefa ou após duas execuções Opus 5.5 do mesmo briefing falharem na checagem. `medium` abaixa cada papel um nível e `small` abaixa dois, e os dois gastam menos tokens.

Você só sobrescreve o que importa. Um papel sem linha na regra mantém o padrão da skill. Para restaurar um padrão, apague a linha desse papel. Uma nova execução do `/setup-pstack` mantém todo papel cujo modelo difere do padrão. Quando um padrão muda, uma regra escrita antes da mudança ainda fixa o padrão antigo até você rodar `/setup-pstack` de novo. Essa execução substitui os padrões antigos que reconhece, diz quais, e mantém suas outras escolhas. Para largar um pin que ela não reconhece, apague a linha do papel ou o arquivo primeiro.

Talvez você se pergunte o que acontece se usar Auto. Defina um papel como `inherit-parent` ou `auto` e o pstack omite o campo `model` do subagente, então o subagente herda o modelo do chat pai. Os dois valores significam a mesma coisa, e nenhum é slug de modelo. Para um papel de painel o valor é uma lista, e um subagente roda por entrada, então o tamanho da lista define o tamanho do painel. A configuração também define `swarm workers`, o modelo padrão de todo worker do `/swarm`, a menos que uma corrida nomeie um modelo para cada braço.

## Aceite as ofertas de verificação, ou não

No fim da configuração, o `/setup-pstack` procura um jeito de provar o comportamento do app no seu projeto, seja uma skill `verify-*`, seja um harness existente. Se não achar nenhum dos dois, ele oferece uma vez gerar um com o [`/create-verification-skill`](../../skills/create-verification-skill/SKILL.md).

Diga sim e ele escreve `.cursor/skills/verify-<app>/`, uma skill local do projeto que ensina agentes a dirigir seu app como um usuário faria. Ele prova que a skill funciona uma vez antes de entregá-la. Diga não e a configuração segue. Você pode rodar `/create-verification-skill` sozinho a qualquer momento. [Verifique e entregue](./06-verify-and-ship.md#create-a-project-verification-skill) cobre isso a fundo.

Se você é novo no pstack, diga sim. Um agente que consegue checar o próprio trabalho segue até a checagem passar. Um agente que não consegue devolve todo resultado para você checar na mão. De tudo neste guia, a skill de verificação é a que mais compensa.

Depois a configuração oferece uma vez gerar uma skill de verificação formal com o [`/create-formal-verification-skill`](../../skills/create-formal-verification-skill/SKILL.md), a menos que o projeto já tenha uma. Testes checam os casos que alguém lembrou de escrever. Uma skill de verificação formal checa uma propriedade chave do seu código, como "um job nunca roda duas vezes ao mesmo tempo", em todo caso dentro de um escopo declarado. Diga sim quando seu código tem concorrência, retentativas, máquina de estados ou um parser, onde um bug custa caro e os testes raramente alcançam o caso que quebra. Caso contrário diga não, e rode depois se isso mudar. [Verifique e entregue](./06-verify-and-ship.md#prove-a-property-with-a-formal-verification-skill) cobre isso.

Depois da configuração, abra um chat novo. A regra de modelo vale para sessões novas.

## Mantenha o custo sob controle

O pstack gasta tokens extras em subagentes e painéis de revisão. Esse é o preço do rigor. Para gastar menos:

- Rode `/setup-pstack` de novo e escolha um orçamento de raciocínio menor ou modelos mais baratos. Um modelo forte no chat principal com modelos mais baratos e rápidos nos papéis de código é uma boa divisão.
- Defina um papel como `auto` ou `inherit-parent` para ele rodar no modelo do próprio chat.
- Encurte a lista de um painel. Cada entrada roda um subagente.
- Guarde o `/poteto-mode` para trabalho que precisa de rigor. Uma edição pequena e óbvia não precisa.

## Rode sua primeira tarefa

Escolha algo real mas pequeno, e descreva do jeito que você descreveria a um colega:

```text
/poteto-mode add a --json flag to this command. text output stays byte-identical. verify both.
```

Olhe a lista de tarefas. Os primeiros itens são os passos do playbook correspondente copiados ali, o playbook Feature neste prompt.

A partir daqui você pode digitar follow-ups normais. Para manter o `/poteto-mode` ligado no chat inteiro, escolha ele no menu `/` com Option+Enter (Mac) ou Alt+Enter (Windows) em vez de Enter. Isso o transforma num [Custom Mode](https://cursor.com/docs/skills#using-a-skill-as-a-custom-mode), que fica em contexto em todo turno até você sair dele. Custom Modes estão disponíveis na Agents Window e na CLI. Enter simples anexa a skill a uma mensagem, e ela se apaga conforme o chat avança.

Próximo: [Roteie o trabalho pelo `/poteto-mode`](./02-poteto-mode.md).
