# Renderizar um gráfico

Renderize cada arquivo `.mmd` que você mostrar com o pacote npm `pele@1.9.0`, e entregue o SVG ao usuário. Instale o Pele uma vez, fora do repositório. Escreva cada SVG ao lado do arquivo `.mmd`, com o mesmo nome. A API fica em https://pele.run/api. Passe `{ limit: 1_000_000, outputLimit: 8_000_000, maxEdges: 10_000 }` como opções de `render`. Os padrões do Pele são menores do que um gráfico de 500 estados pode ser. Quando o Pele lança um `PeleError`, mostre ao usuário o texto Mermaid e o erro.

## Cores

O Pele ignora `theme` e `themeVariables` do Mermaid. Defina as variáveis CSS `--pele-*` dele em `.pele`. https://pele.run/theming traz a lista. O comando dá a cada estado a classe de cor do grupo dele, como `class sN teal`. Ele marca cada estado ruim com `class sN breaks` ou `class sN mayBreak`, cada estado no caminho mais curto até um bug com `class sN bugPath`, e cada estado que um contraexemplo de PROPERTY repete para sempre com `class sN repeats`. O Pele põe essas classes no nó do estado. Pinte as classes com CSS.

O arquivo `.mmd` tem uma linha `classDef` para `breaks` e para `mayBreak`, para que qualquer visualizador Mermaid mostre as marcas de bug. Ele não tem `classDef` para as cores de grupo, `bugPath` ou `repeats`. O Pele escreve a cor de cada `classDef` como atributo `style` no nó, e um atributo `style` vence uma regra CSS normal. Por isso as regras das marcas abaixo usam `!important`. Sem isso, um tema escuro mantém as cores claras das marcas. Uma marca de bug define o preenchimento e o texto, e `bugPath` e `repeats` definem a borda. Ponha as regras de marca depois das cores de grupo, para uma marca aparecer sobre a cor do grupo.

Estas cores são sugestões. São as cores da skill antiga. Ponha-as num elemento `<style>` no começo de cada SVG. Cada SVG então segue a configuração clara ou escura do visualizador.

```css
.pele {
  --pele-bg: #ffffff;
  --pele-fg: #1c1c1c;
  --pele-muted: #656565;
  --pele-line: #808080;
  --pele-surface: #fcfcfc;
  --pele-surface-alt: #f1f1f1;
  --pele-border: #c9c9c9;
}
.pele .blue rect { fill: #c3e0ff; }
.pele .orange rect { fill: #f0d3bf; }
.pele .purple rect { fill: #e3d5ff; }
.pele .yellow rect { fill: #f0dcbf; }
.pele .teal rect { fill: #bfe3df; }
.pele .tan rect { fill: #e5d9ce; }
.pele .pink rect { fill: #ffcbe6; }
.pele .breaks rect {
  fill: #ffecee !important;
  stroke: #e72e3b !important;
  stroke-width: 2.5px !important;
}
.pele .breaks text {
  fill: #cd2834 !important;
}
.pele .mayBreak rect {
  fill: #fff3e6 !important;
  stroke: #e8730c !important;
  stroke-width: 2.5px !important;
}
.pele .mayBreak text {
  fill: #b85a06 !important;
}
.pele .bugPath rect {
  stroke: #1680f3 !important;
  stroke-width: 2.5px !important;
}
.pele .repeats rect {
  stroke: #9b35d6 !important;
  stroke-width: 2.5px !important;
  stroke-dasharray: 6 3;
}
@media (prefers-color-scheme: dark) {
  .pele {
    --pele-bg: #1f1f1f;
    --pele-fg: #f5f5f5;
    --pele-muted: #acacac;
    --pele-line: #939393;
    --pele-surface: #121212;
    --pele-surface-alt: #2c2c2c;
    --pele-border: #4e4e4e;
  }
  .pele .blue rect { fill: #1b3857; }
  .pele .orange rect { fill: #573117; }
  .pele .purple rect { fill: #3c2e57; }
  .pele .yellow rect { fill: #573d17; }
  .pele .teal rect { fill: #174641; }
  .pele .tan rect { fill: #3d3127; }
  .pele .pink rect { fill: #57233e; }
  .pele .breaks rect {
    fill: #2e2021 !important;
    stroke: #cf2b35 !important;
  }
  .pele .breaks text {
    fill: #ff6463 !important;
  }
  .pele .mayBreak rect {
    fill: #2e2619 !important;
    stroke: #d0700f !important;
  }
  .pele .mayBreak text {
    fill: #ffa257 !important;
  }
  .pele .bugPath rect {
    stroke: #116dd2 !important;
  }
  .pele .repeats rect {
    stroke: #9a3fcf !important;
  }
}
```
