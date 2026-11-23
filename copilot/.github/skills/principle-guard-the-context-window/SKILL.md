---
name: principle-guard-the-context-window
description: "Aplique quando o contexto estiver enchendo: saídas grandes, arquivos longos, leituras repetidas, planejamento de leque. Roteie o volume para subagentes; mantenha resumos na thread principal, não payloads crus."
---

# Proteja a Janela de Contexto

**Por quê:** transbordo de contexto degrada o raciocínio, cria artefatos de compressão, e trava o progresso.

**Padrão:**
- **Dimensione as fases e limite o escopo.** Limite arquivos por fase, defina orçamentos de turno, conte os custos de mecanismo.
