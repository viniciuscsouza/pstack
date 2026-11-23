### Forense de runtime

**O diagnóstico é seu. Instrumente o processo ao vivo, não teorize a partir do código.** A entrega é um diagnóstico citado, não uma correção.

1. Capture o sinal ao vivo na superfície correspondente pela skill de controle. Um profile de CPU para um processo girando, um snapshot de heap para um vazamento, um trace de CDP para um defeito visual. Um artefato real, não um palpite.
2. Reduza o artefato à prova cabal. A função no caminho quente, a cadeia de retenção do objeto vazado até uma raiz de GC, o laço disparando sem entrada. Faça parse de artefatos grandes num subagente (a skill de princípio **guard-the-context-window**), mantenha o achado reduzido na thread principal. Escolha o nível de raciocínio de cada subagente para o trabalho, conforme a seção Subagentes.
3. Prove o mecanismo antes de acreditar nele. Injete instrumentação via eval de CDP no processo em execução, ou faça hotfix do código vivo sem recarregar, para confirmar a hipótese por pouco custo.
4. Mapeie o achado de volta ao código. Arquivo, símbolo, a linha que aloca ou agenda.
5. O checkpoint de throughput fica numa linha. `throughput checkpoint: n/a, read-only forensics`.

**Resposta:** o sinal capturado, o achado reduzido, como você provou o mecanismo, o local no código, caminhos dos artefatos. Nenhuma correção, a menos que peçam. Devolva ao Bug fix ou ao Perf quando a causa for conhecida.
