---
name: tdd
description: "Use só quando o usuário pedir TDD explicitamente, um teste que falha, ou um teste de regressão, OU quando o bug tiver um alvo de teste local óbvio e barato. Pule quando o caminho de teste for incerto, caro, cheio de integração, ou não pedido."
disable-model-invocation: true
---

# Correção de Bug com TDD

Ao corrigir um bug com caminho de teste claro e barato, torne o comportamento quebrado executável antes de mudar o código de produção. O objetivo é um teste de regressão focado que falha antes da correção e passa depois dela.

Não force um teste impraticável. Se o teste disponível exigir montagem ampla de harness, mocks frágeis, infraestrutura de ponta a ponta lenta, estado só de produção, passos de reprodução vagos, ou muita mexida em fixture não relacionada, pule o teste novo e use a verificação útil mais próxima.

## Fluxo

1. **Entenda o bug.** Identifique o comportamento pretendido, o comportamento atual, o caminho afetado, e a menor reprodução observável.
2. **Escolha a checagem executável mais estreita.** Prefira o teste de unidade, componente, integração ou regressão mais próximo que aquele caminho de código já usa. Se nenhum caminho de teste prático é óbvio, não crie um do zero só para satisfazer o fluxo.
3. **Escreva o teste que falha primeiro.** Adicione o menor teste focado que teria pego o bug. O teste deve codificar o comportamento pretendido, não espelhar a implementação atual.
4. **Rode o teste novo antes de corrigir.** Confirme que ele falha pelo motivo pretendido. Se ele passa ou falha por motivo não relacionado, corrija o teste ou a reprodução antes de editar a implementação.
5. **Corrija o bug.** Faça a menor mudança de produção que satisfaz o comportamento pretendido preservando os contratos próximos.
6. **Rode o teste de regressão de novo.** Confirme que o teste agora passa.

## Se um Teste que Falha For Impraticável

Use a checagem de regressão executável mais próxima. Script direcionado, comando de reprodução manual, automação de browser, comparação de snapshot, asserção de log, ou checagem de integração focada.

Prefira nenhum teste novo a um teste ruim. Um teste ruim testa majoritariamente mocks, codifica detalhes da implementação atual, depende de timing ou de estado global não relacionado, precisa de infraestrutura cara para uma correção pequena, ou seria apagado logo depois de provar a correção.

## Guardas

- Não mude testes só para casar com uma implementação errada.
- Não enfraqueça asserções existentes a menos que o comportamento esperado tenha mudado de verdade e o motivo esteja claro.
- Mantenha o teste de regressão focado no bug. Evite mexida ampla em fixture ou expansão de cobertura não relacionada.
- Se o bug é intermitente, torne o teste determinístico onde for possível e documente o sinal travado.
- Se o bug expõe uma classe mais ampla de falhas, entregue primeiro o caminho de regressão focado, depois considere cobertura irmã adicional.

## Resposta Final

Reporte a evidência, não só o resultado.

- Nomeie o teste que falha antes ou a checagem executável, e a falha que ela produziu.
- Nomeie a execução do teste que passa depois e qualquer validação próxima feita.
- Se a evidência de falha antes não pôde ser demonstrada, diga por quê e descreva a checagem de regressão mais próxima usada no lugar.
