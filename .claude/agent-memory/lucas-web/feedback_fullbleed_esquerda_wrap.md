---
name: fullbleed-esquerda-wrap
description: Foto full-bleed ate borda esquerda da viewport dentro de container centralizado (max-width + padding)
metadata:
  type: feedback
---

Para sangrar um elemento ate a borda ESQUERDA da viewport quando ele esta dentro de um `.wrap` centralizado (max-width:1200px; padding:0 28px), o offset da borda do conteudo ate a viewport e `max(28px, 50vw - 572px)` — ou seja `(50vw - 600px) + 28px de padding`. Esquecer o padding do wrap deixa a foto parada na borda interna do container (erro que tive: usei `50vw-600px`, faltavam 28px).

**Why:** Margin-left negativo dentro de um grid track `1fr` NAO sangra — o track confina o item. Solucao que funcionou: `.about-photo{position:absolute;left:calc(-1 * max(28px,50vw-572px));top:0;bottom:0;width:calc(50% + offset - gap/2)}` com `.about-grid{position:relative;min-height}` como containing block, e `.about-content{grid-column:2}` para o texto ocupar so a coluna direita.

**How to apply:** Hero split foto-esquerda + texto-direita "grudado na esquerda". Border-radius so do lado interno (`0 26px 26px 0`). `overflow-x:clip` no wrapper pai. No mobile resetar tudo (`position:relative;left:auto;width:auto`, radius cheio, `grid-column:auto`) para empilhar. Validar overflow 0 (scrollWidth==clientWidth) no Playwright. Relacionado a [[feedback_marquee_diagonal_x_fullbleed]].

Quando o cliente ainda relata folga residual a esquerda mesmo com a formula correta: adicionar **+2px de sangria de seguranca** ao offset e a largura (`left:calc(-1*(max(28px,50vw-572px)+2px))`, `width:calc(50% + max(28px,50vw-572px)+2px - 30px)`) e por `overflow-x:clip` no proprio bloco pai (ex: `.about`). Os 2px cobrem arredondamento subpixel e a scrollbar (que faz `50vw` contar a largura da barra); o clip corta o excesso sem criar scroll-x. Validado a 920-1440px: borda esquerda da foto em x=0 em todas; overflow-x:no de 375 a 1440.
