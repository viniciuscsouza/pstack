---
name: principle-separate-before-serializing-shared-state
description: "Aplique quando atores concorrentes podem escrever no mesmo arquivo, branch, chave ou objeto de estado. Elimine o compartilhamento primeiro. Serialize estruturalmente só quando um escritor compartilhado é invariante de verdade."
---

# Separe Antes de Serializar Estado Compartilhado

Quando atores concorrentes podem escrever no mesmo arquivo, branch, chave ou objeto, pergunte primeiro se eles precisam de um objeto compartilhado ou se estão publicando fatos independentes. O padrão é dar a cada ator o próprio arquivo, chave, branch ou diretório de estado, e juntar só onde o estado é lido. Dois workers escrevendo o próprio campo num único `state.json` ainda é mutação compartilhada.

Só quando um alvo de escrita compartilhado é invariante de verdade, serialize o acesso estruturalmente (um lockfile, um escritor único, fases sequenciais, compare-and-swap). Instruções não são controle de concorrência, e "precisamos de um lock" é um cheiro para checar primeiro.
