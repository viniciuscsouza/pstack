---
name: principle-outcome-oriented-execution
description: "Aplique durante reescritas e migrações planejadas com fronteiras de fase explícitas. Convirja para a arquitetura alvo; não preserve estados intermediários suaves com código de compatibilidade descartável."
---

# Execução Orientada a Resultado

Otimize para o estado final pretendido e verificável, não para preservar estados intermediários suaves.

**Por quê:** Manter todo passo intermediário estável muitas vezes cria código de compatibilidade temporário que vira dívida de longo prazo. Convirja para a arquitetura alvo e prove a correção em fronteiras de verificação explícitas.

**Regra central:**
- Priorize a integridade do estado final sobre a estabilidade transitória
- Quebra intermediária é aceitável quando planejada, delimitada, e reversível

**Guardrails:**
- Use isto para reescritas e migrações planejadas com fronteiras de fase explícitas
- Declare onde a quebra temporária é aceitável
- Mantenha checagens de alto sinal nas áreas tocadas durante a migração
- Exija verificação estática e de runtime completa na conclusão do plano
