### Trace forensics

**O diagnóstico a partir do artefato é seu.**

Diferente de **Runtime forensics**, que instrumenta o processo vivo. Aqui a captura já existe. O artefato é um conjunto de dados fixo, leia, não rode de novo. Mantenha o ferramental genérico. Um parser de DevTools ou de trace para cpuprofile e `.json.gz`, um editor de texto para um spindump, seu ferramental de heap para um heapsnapshot.

1. Identifique o formato e carregue com a ferramenta certa. Faça parse de artefatos grandes num subagente (a skill **principle-guard-the-context-window**) e mantenha o achado reduzido na thread principal. Escolha o nível de raciocínio de cada subagente para o trabalho, conforme a seção Subagentes.
2. Transforme o artefato cru numa forma que você consiga consultar. Despeje o trace ou o heap snapshot em sqlite, uma linha por amostra, frame ou nó. Chegue à forma consultável antes de ler.
3. Estreite até a causa. Consulte os frames que seguram mais tempo e caminhe pela árvore de chamadas até o caminho quente. Para um vazamento, siga a cadeia de retenção do objeto vazado até uma raiz de GC. Para um spindump, ache a thread presa em CPU ou bloqueada e o motivo da espera dela.
4. Atribua ao código-fonte. Mapeie o frame quente para arquivo, símbolo e linha pelos símbolos do próprio artefato. Um frame sem mapeamento de código ainda não é diagnóstico. Resolva os símbolos, ou diga com clareza que o artefato não os carrega.
5. Se você tem uma captura pareada, faça o diff dos artefatos de antes e depois para confirmar. Senão, marque o achado como a hipótese mais forte que o artefato sustenta, não como causa confirmada.
6. Devolva um diagnóstico citado, sem correção a menos que peçam. Roteie para Bug fix ou Perf issue quando a causa for conhecida. O checkpoint de throughput fica numa linha: `throughput checkpoint: n/a, read-only forensics`.

**Resposta:** o artefato e o formato, o achado reduzido, a localização no código, os caminhos dos artefatos, e se uma captura pareada confirmou.
