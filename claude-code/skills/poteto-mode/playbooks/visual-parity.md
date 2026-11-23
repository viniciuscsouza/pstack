### Visual parity

**Você é dono da equivalência exata em pixel. A linha de base é a especificação. Você não mexe nela.** A equivalência é verificada por diff de imagem, não a olho.

1. Estabeleça a linha de base antes de qualquer migração. Um harness de regressão visual que tira screenshot do componente atual em todos os estados, mais o alvo ao casar duas implementações. Sem linha de base, sem alegação de paridade. Pré-requisito bloqueante, não follow-up.
2. Cláusulas anti-atalho, declaradas e mantidas. Sem modificação no harness, nenhuma adulteração da linha de base, nenhuma reestruturação de componente para fazer um diff passar. Se ela parecer errada, pare e pergunte. Não edite.
3. Migre um componente por vez. Paralelize entre worktrees, um dono por componente (a skill de princípio **separate-before-serializing-shared-state**). Primitivos compartilhados migram primeiro como fase bloqueante.
4. Verifique cada componente contra a linha de base por diff de imagem na superfície correspondente, pela skill de controle. Um diff diferente de zero é falha. Investigue o delta de pixel. `/loop` por componente até o diff zerar.
5. Rode **Opening a PR** por componente ou por lote seguro.

**Resposta:** componentes migrados, o resultado do diff de cada um, o local do harness de linha de base, o que falta.
