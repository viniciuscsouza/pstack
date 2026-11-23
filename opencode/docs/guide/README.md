# O guia do pstack

O pstack funciona melhor quando você para de microgerenciar o agente. Você descreve o que quer e como vai saber que está pronto. `/poteto-mode` escolhe o playbook, roda as outras skills conforme os passos pedem, e mostra a evidência. Este guia ensina esse hábito com prompts realistas.

O que você vai aprender:

1. [Configure o pstack](./01-setup.md). Instale o plugin e escolha seus modelos.
2. [Roteie o trabalho pelo `/poteto-mode`](./02-poteto-mode.md). Dê um objetivo e veja ele escolher um playbook.
3. [Entenda o código](./03-understand.md). Uma investigação somente leitura, depois `/how`, `/why`, `/teach` e `/recall` antes de editar qualquer coisa.
4. [Desenhe a mudança](./04-design.md). `/architect`, `/arena`, `/swarm`, `/interrogate`, protótipos e planos antes de o código travar uma forma.
5. [Construa e limpe a mudança](./05-build-and-clean.md). Os playbooks de construção, `/tdd`, `/unslop` e `/no-comments`.
6. [Verifique e entregue](./06-verify-and-ship.md). Prove o comportamento no app real, verifique formalmente o que testes raramente alcançam, valide números com `/benchmark-checklist`, depois abra um PR focado e leve ele até o merge.
7. [Rode trabalho enquanto dorme](./07-overnight.md). Confiança antes de laços, um contrato noturno, um log de decisões que você pode auditar, e Projetos e automações que escalam além de um agente.
8. [Direcione com nomes de princípio](./08-principles.md). Os 24 nomes que redirecionam um agente no meio da tarefa.
9. [Faça do seu jeito](./09-make-it-yours.md). Seu próprio modo, `/correct` para erros repetidos, e como testar uma mudança de skill.
10. [Receitas e armadilhas](./10-recipes-and-pitfalls.md). Prompts para copiar e erros para pular.

Leia as páginas em ordem na primeira vez. Depois disso, cada página fica sozinha.

Quando você travar, ou não souber qual skill se encaixa, digite [`/poteto-help`](../../.opencode/skills/poteto-help/SKILL.md) com a sua pergunta:

```text
/poteto-help which skill should i use to review this branch?
```

Ele responde, entrega um prompt para você enviar, e linka a skill ou página do guia de onde a resposta veio. Não começa o trabalho, porque uma execução do pstack gasta tokens reais, então você envia o prompt quando estiver pronto. Ele roda só quando você digita.

## Se você só lembrar de uma coisa

Dê ao agente um objetivo e um jeito de checar ele, com as suas palavras:

```text
/poteto-mode the export writes duplicate rows when a retry lands mid-run. repro first, then fix and verify.
```

Você não precisa nomear um playbook nem listar skills. "repro first" e um resultado checável são todo o sinal de roteamento que `/poteto-mode` precisa. Ele casa com o playbook Bug fix, copia os passos para uma lista de tarefas, e chama as skills certas conforme cada passo dispara.

Próximo: [Configure o pstack](./01-setup.md).
