---
name: principle-migrate-callers-then-delete-legacy-apis
description: "Aplique ao introduzir uma API interna nova enquanto chamadores antigos ainda existem. Migre os chamadores e apague a API antiga na mesma onda em vez de preservar camadas de compatibilidade."
disable-model-invocation: true
---

# Migre os Chamadores, Depois Apague as APIs Legadas

- Atualize os testes para afirmar o contrato novo, e apague os testes que só protegem detalhes de implementação pré-refatoração

**Quando isso se aplica:**
- Nenhum usuário externo depende de compatibilidade retroativa
- O projeto consegue absorver mudanças disruptivas coordenadas
- A API nova faz parte de uma iniciativa de simplificação ou refatoração
