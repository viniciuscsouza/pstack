### Autopilot-stack

Rode o playbook Autopilot-full, mas não faça merge.

1. **No lugar do passo 5, pare no verificado.** Pare quando cada PR está verificada e verde no head atual dela, depois entregue as PRs ao operador. Depois da entrega, ninguém faz push sem o aval do operador. Todo prompt de tick do laço nomeia este arquivo, então cada tick vê a regra de não fazer merge.
2. **Empilhe uma PR só quando ela se baseia em outra PR.** Esta é a única exceção ao passo 3. Baseie a PR no branch da outra PR no lugar do trunk, conforme `playbooks/opening-a-pr.md`. No lugar do rebase do passo 2 sobre o trunk, cada PR faz rebase sobre o branch da PR pai, e a raiz da pilha faz rebase sobre o trunk. Um head novo na PR de baixo reempilha as PRs acima dela, e uma PR cujo `git patch-id` o reempilhamento não muda mantém o veredito.

**Resposta:** liste cada PR com o veredito dela, no lugar do que foi mergeado.
