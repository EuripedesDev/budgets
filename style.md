# Padrão visual das propostas Tech

Guia para montar as próximas propostas comerciais no mesmo estilo de [flora-bandeira/index.html](flora-bandeira/index.html), que é a **referência oficial**. Na dúvida, copie o trecho de lá.

## 1. Estrutura de arquivos

- Uma pasta por cliente, com nome em kebab-case: `flora-bandeira/`, `stampo/`.
- Cada proposta é um único `index.html` autocontido: CSS inline no `<style>`, ícone em base64 e nenhum JS.
- Única dependência externa: a fonte Montserrat, carregada do Google Fonts.
- Para começar uma proposta nova, **copie** `flora-bandeira/index.html` e troque o conteúdo. Não mexa no `<style>`, a não ser para adicionar um componente novo (ver seção 8).

## 2. Identidade

| Item | Valor |
|---|---|
| Marca | **Tech** |
| Fonte | Montserrat 400 / 500 / 600 / 700 / 800 |
| Cor de destaque | dourado em gradiente |
| Ícone | PNG 108×160 em base64 (o mesmo usado na nav, no hero e no fechamento da Flora Bandeira) |
| Tema | fixo claro/escuro alternado (`color-scheme: light`); **não** segue o modo escuro do sistema |
| Idioma | `lang="pt-BR"` |

### Tokens de cor (`:root`)

```css
--black:#000000;--ink:#1d1d1f;--tile-dark:#161617;--tile-dark-2:#1d1d1f;
--light:#f5f5f7;--white:#ffffff;
--text-dark:#1d1d1f;--text-dark-2:#6e6e73;--text-light:#f5f5f7;--text-light-2:#a1a1a6;
--hair-dark:#d2d2d7;--hair-light:#333336;
--gold-hi:#f0a863;--gold-mid:#c48a45;--gold-lo:#8a5e24;--gold-text:#8a5a1c;
--gold-grad:linear-gradient(120deg,#ffd9a8 0%,#f0a863 35%,#c48a45 65%,#8a5e24 100%);
```

Regras de uso do dourado:
- `--gold-grad` com a classe `.gold` (texto) vai em títulos de destaque, preços e numerais. Nunca em texto corrido.
- `--gold-text` é o rótulo (eyebrow/tag) sobre fundo claro, porque tem contraste suficiente.
- `--gold-hi` é o rótulo sobre fundo escuro.
- `--gold-mid` é para bullets e ícones de check.
- Fora os tokens, nenhuma cor nova. Precisando de uma, adicione como token.

## 3. Tipografia

| Elemento | Tamanho | Peso |
|---|---|---|
| Corpo | 17px, line-height 1.5, letter-spacing -.01em | 400–500 |
| `h1` do hero | `clamp(46px, 9vw, 96px)` | 800 |
| `h2` de seção | `clamp(40px, 7vw, 72px)` | 700 |
| Preço principal | `clamp(64px, 13vw, 140px)` | 800 |
| Eyebrow / tag / cat | 13–17px | 600–700 |

- Os títulos levam `text-wrap: balance`, letter-spacing -.025em e line-height 1.07.
- Valores numéricos levam a classe `.num` (tabular-nums).
- Para quebrar títulos de propósito, use `<br>` (ex.: "Duas premissas.<br>Zero surpresas.").

## 4. Layout

- Container `.in` com no máximo 1024px de largura e gutter lateral de 22px (respeitando safe-area).
- Seções `.sec` com `padding-block: 110px` (72px no mobile).
- O fundo das seções alterna para criar ritmo: `.dark` (preto), `.light` (#f5f5f7) e `.white`. Nunca duas seções seguidas com o mesmo fundo.
- Os blocos têm cantos arredondados de 28px (22px no mobile); os steps usam 24px.
- Breakpoints usados: 860px, 820px (nav), 760px, 640px, 560px e 480px. Todo grid cai para 1 coluna no celular.
- Movimento: só o glow do ícone no hero e o scroll suave, ambos desligados em `prefers-reduced-motion`.

## 5. Ordem das seções

Siga esta sequência. Pode remover a seção que não se aplicar, mas sem reordenar.

| # | Seção | Fundo | id | Componente |
|---|---|---|---|---|
| 0 | Nav fixa | escura translúcida | — | `.lnav` com ícone, nome do cliente, links e pill "Ver valores" |
| 1 | Hero | dark | `topo` | `.hero` com ícone, kicker "Proposta comercial · Tech", h1 `.gold`, `.sub`, `.ctas`, `.meta` (cliente, elaborado por, data, validade) |
| 2 | Premissas | light | `premissas` | `.duo` com dois `.tile` (um `.w`, um `.d`) |
| 3 | O que muda | dark | — | `.trio` com três `.big.gold` |
| 4 | Escopo / Módulos | white | `modulos` | `.bento` com `.b`, `.b.s2`, `.b.dk` e `.b.opt` (opcionais) |
| 5 | Design / Princípios | dark | `design` | `.princ` com 6 parágrafos que começam por `<b>` |
| 6 | Cronograma | light | `etapas` | `.steps` com `.step` (número `.n.gold`, `.wk` com semanas, `h4`, `p`) |
| 7 | Valores | dark | `valores` | `.price`, `.payopts` (2 opções), `.nego`, tabela por etapa, `.fine` |
| 8 | Incluso / Não incluso | light | `incluso` | `.lists` com check dourado e X cinza, mais `.ask` (pontos a alinhar) |
| 9 | Fechamento | dark | — | `.end` com ícone, "Vamos começar." e 4 próximos passos |
| 10 | Rodapé | light | — | `footer` com autoria e validade |

Os links da nav devem bater com os `id` das seções existentes.

## 6. Componentes (cola rápida)

### Cabeçalho de seção
```html
<div class="head">
  <span class="eyebrow">Rótulo curto</span>
  <h2 id="h-xxx">Frase curta.<br>Com ponto final.</h2>
  <p>Subtítulo opcional de até ~40 caracteres por linha.</p>
</div>
```
Toda `<section>` tem `aria-labelledby` apontando para o `id` do seu `h2`.

### Bloco de módulo
```html
<article class="b">            <!-- .s2 = ocupa 2 colunas · .dk = escuro · .opt = opcional tracejado -->
  <span class="cat">Categoria</span>   <!-- em .opt troque por <span class="badge">Opcional</span> -->
  <h4>Nome do módulo.</h4>
  <p>Uma frase de benefício.</p>
  <ul><li>Item</li><li>Item</li></ul>
</article>
```
No bento de 3 colunas, monte linhas que fechem 3 (ex.: `s2 + 1`, ou `1 + 1 + 1`). Os blocos `.dk` servem para os módulos mais importantes.

### Etapa
```html
<article class="step"><span class="n gold">1</span><span class="wk">Semanas 1–3</span><h4>Título</h4><p>Descrição.</p></article>
```

### Opção de pagamento
```html
<article class="payopt">
  <span class="tag">À vista</span>
  <span class="v num">R$ 0.000 <em>+ complemento</em></span>
  <p>Explicação.</p>
</article>
```

### Linhas da tabela de valores
- `<tr>`: linha normal.
- `tr.total`: total, com valor `.v.gold`.
- `tr.sh`: subtítulo (ex.: "Opcional, contratado à parte").
- `tr.opt`: item opcional, em tom apagado.

Ícones de check e X: copie os SVGs inline da seção "Incluso" (círculo `#c48a45` com check branco; círculo `#d2d2d7` com X `#6e6e73`).

## 7. Texto e tom

- Português do Brasil, tratando o cliente por "você". Direto, sem jargão técnico desnecessário.
- Títulos curtos e afirmativos, **terminando em ponto final** ("Valores.", "Tudo o que a operação precisa.").
- Itens de lista sem ponto final.
- Cada módulo diz **o benefício** no `<p>` e o detalhe nos `<li>`.
- Valores no formato `R$ 14.900` (ponto de milhar, sem centavos quando redondo).
- Datas no formato `DD/MM/AAAA`. A validade padrão é de **15 dias**.
- Sempre incluir:
  - premissas que evitam surpresas (o que é responsabilidade do cliente)
  - a lista "Não incluso" explícita
  - "Condições negociáveis" e a possibilidade de contratar por etapa
  - aviso de que terceiros (hospedagem, domínio, etc.) são pagos pelo cliente, quando houver
  - "Para alinhar antes do fechamento", com as perguntas em aberto
  - 4 próximos passos no fechamento

## 8. Adaptações por tipo de proposta

A estrutura acima nasceu de uma proposta de **sistema**. Para outros serviços, mantenha o esqueleto e troque o conteúdo das seções:

| Seção | Sistema (ex.: Flora Bandeira) | Consultoria / mapeamento (ex.: fábrica) |
|---|---|---|
| O que muda | antes e depois do sistema | antes e depois da operação (visibilidade, gargalo, lead time) |
| Módulos | funcionalidades | entregáveis (VSM, matriz produto×processo, estudo de tempos, plano de ação) |
| Design | princípios de interface | método (ida a campo, cronoanálise, TOC, validação com a equipe) |
| Etapas | semanas de desenvolvimento | fases de levantamento, análise e apresentação |

Se precisar de um componente novo:
1. Crie em `flora-bandeira/index.html` só se for útil para lá também; caso contrário, crie na proposta nova.
2. Use apenas os tokens existentes, raio de 22–28px e breakpoints já usados.
3. Documente o componente aqui na seção 6.

## 9. Checklist antes de enviar

- [ ] `<title>` no formato "Proposta Nome do Cliente"
- [ ] Nome do cliente igual na nav, no `.meta`, no rodapé e nos textos
- [ ] Data e validade atualizadas no hero e no rodapé
- [ ] A soma da tabela por etapa bate com o total e com o preço principal
- [ ] Os valores de entrada e parcelamento batem com o total (ex.: 30% de entrada)
- [ ] Links da nav e `aria-labelledby` apontando para ids existentes
- [ ] Fundos das seções alternando
- [ ] Testado no celular (largura ~375px) sem rolagem horizontal
- [ ] Nenhum resto de texto da proposta anterior (busque pelo nome do cliente antigo)
