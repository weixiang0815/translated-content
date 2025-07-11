---
title: padding-inline-end
slug: Web/CSS/padding-inline-end
---

{{CSSRef}}{{SeeCompatTable}}

La propriété **`padding-inline-end`** définit le décalage avec la fin de la zone de remplissage d'un élément, selon le mode d'écriture, la directionnalité et l'orientation du texte. Elle correspond à la propriété {{cssxref("padding-top")}}, {{cssxref("padding-right")}}, {{cssxref("padding-bottom")}} ou {{cssxref("padding-left")}} selon les valeurs définies pour {{cssxref("writing-mode")}}, {{cssxref("direction")}} et {{cssxref("text-orientation")}}.

Les autres parties « logiques » du remplissage sont définies grâce aux propriétés {{cssxref("padding-block-start")}}, {{cssxref("padding-block-end")}} et {{cssxref("padding-inline-start")}}.

{{InteractiveExample("CSS Demo: padding-inline-end")}}

```css interactive-example-choice
padding-inline-end: 20px;
writing-mode: horizontal-tb;
```

```css interactive-example-choice
padding-inline-end: 20px;
writing-mode: vertical-rl;
```

```css interactive-example-choice
padding-inline-end: 5em;
writing-mode: horizontal-tb;
direction: rtl;
```

```html interactive-example
<section id="default-example">
  <div class="transition-all" id="example-element">
    <div class="box">
      Far out in the uncharted backwaters of the unfashionable end of the
      western spiral arm of the Galaxy lies a small unregarded yellow sun.
    </div>
  </div>
</section>
```

```css interactive-example
#example-element {
  border: 10px solid #ffc129;
  overflow: hidden;
  text-align: left;
}

.box {
  border: dashed 1px;
  unicode-bidi: bidi-override;
}
```

## Syntaxe

```css
/* Valeurs de longueur */
/* Type <length>       */
padding-inline-end: 10px;
padding-inline-end: 1em;

/* Valeurs de proportions */
/* Type <percentage>      */
padding-inline-end: 5%; /* Relatif à la largeur du bloc englobant */

/* Valeurs globales */
padding-inline-end: inherit;
padding-inline-end: initial;
padding-inline-end: unset;
```

### Valeurs

La propriété `padding-inline-end` peut prendre les mêmes valeurs que la propriété {{cssxref("padding-left")}}.

## Définition formelle

{{CSSInfo}}

## Syntaxe formelle

{{CSSSyntax}}

## Exemples

### HTML

```html
<div>
  <p class="exemple">Texte d'exemple Lorem y psoum</p>
</div>
```

### CSS

```css
div {
  background-color: yellow;
  width: 120px;
  height: 120px;
}

.exemple {
  writing-mode: vertical-lr;
  padding-inline-end: 20px;
  background-color: #c8c800;
}
```

### Résultat

{{EmbedLiveSample("Exemples", 140, 140)}}

## Spécifications

{{Specifications}}

## Compatibilité des navigateurs

{{Compat}}

## Voir aussi

- Les propriétés physiques correspondantes :
  - {{cssxref("padding-top")}},
  - {{cssxref("padding-right")}},
  - {{cssxref("padding-bottom")}},
  - {{cssxref("padding-left")}}

- {{cssxref("writing-mode")}},
- {{cssxref("direction")}},
- {{cssxref("text-orientation")}}
