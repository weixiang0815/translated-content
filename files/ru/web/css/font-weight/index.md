---
title: font-weight
slug: Web/CSS/font-weight
---

{{CSSRef}}

[CSS](/ru/docs/Web/CSS) свойство **`font-weight`** устанавливает начертание шрифта. Некоторые шрифты доступны только в нормальном или полужирном начертании.

{{InteractiveExample("CSS Demo: font-weight")}}

```css interactive-example-choice
font-weight: normal;
```

```css interactive-example-choice
font-weight: bold;
```

```css interactive-example-choice
font-weight: lighter;
```

```css interactive-example-choice
font-weight: bolder;
```

```css interactive-example-choice
font-weight: 100;
```

```css interactive-example-choice
font-weight: 900;
```

```html interactive-example
<section id="default-example">
  <p id="example-element">
    London. Michaelmas term lately over, and the Lord Chancellor sitting in
    Lincoln's Inn Hall. Implacable November weather. As much mud in the streets
    as if the waters had but newly retired from the face of the earth, and it
    would not be wonderful to meet a Megalosaurus, forty feet long or so,
    waddling like an elephantine lizard up Holborn Hill.
  </p>
</section>
```

```css interactive-example
section {
  font-size: 1.2em;
}
```

## Синтаксис

```css
font-weight: normal;
font-weight: bold;

/* Relative to the parent */
font-weight: lighter;
font-weight: bolder;

font-weight: 100;
font-weight: 200;
font-weight: 300;
font-weight: 400;
font-weight: 500;
font-weight: 600;
font-weight: 700;
font-weight: 800;
font-weight: 900;

/* Global values */
font-weight: inherit;
font-weight: initial;
font-weight: unset;
```

### Значения

- `normal`
  - : Нормальное начертание. То же, что и `400`.
- `bold`
  - : Полужирное начертание. То же, что и `700`.
- `lighter`
  - : Изменяет начертание относительно насыщенности родительского элемента (сверхтонкое начертание).
- `bolder`
  - : Изменяет начертание относительно насыщенности родителя элемента (сверхжирное начертание).
- `100`, `200`, `300`, `400`, `500`, `600`, `700`, `800`, `900`
  - : Цифровые значения насыщенности шрифтов, которые позволяют задавать больше, чем нормальное и полужирное начертание.

### Недоступность заданного значения

Если заданное цифровое значение насыщенности недоступно, для определения толщины отображаемого шрифта используется следующий алгоритм:

- Если задано значение выше `500`, будет использовано первое доступное более жирное начертание (если такого не окажется, то первое доступное более светлое).
- Если задано значение менее `400`, будет использовано первое доступное более светлое начертание (если такого не окажется, то первое доступное более жирное).
- Если задано значение `400`, будет применено `500`. Если `500` недоступно, то будет использован алгоритм для подбора значений менее `400`.
- Если задано значение `500`, будет применено `400`. Если `400` недоступно, то будет использован алгоритм для подбора значений менее `400`.

Это означает, что для шрифтов, которые предоставляют только normal и bold начертания, 100-500 normal, и 600-900 bold.

### Значение относительных весов

Когда используется **жирнее** или **светлее**, следующая таблица используется для вычисления абсолютного веса элемента:

| наследуемое значение | `жирнее` | `светлее` |
| -------------------- | -------- | --------- |
| 100                  | 400      | 100       |
| 200                  | 400      | 100       |
| 300                  | 400      | 100       |
| 400                  | 700      | 100       |
| 500                  | 700      | 100       |
| 600                  | 900      | 400       |
| 700                  | 900      | 400       |
| 800                  | 900      | 700       |
| 900                  | 900      | 700       |

### Определение веса имени

Значения от 100 до 900, примерно, соответствуют следующим распространённым именам насыщенности:

| Значение | Общее название                                                  |
| -------- | --------------------------------------------------------------- |
| `100`    | Тонкий (Волосяной) Thin (Hairline)                              |
| `200`    | Дополнительный светлый (Сверхсветлый) Extra Light (Ultra Light) |
| `300`    | Светлый Light                                                   |
| `400`    | Нормальный Normal                                               |
| `500`    | Средний Medium                                                  |
| `600`    | Полужирный Semi Bold (Demi Bold)                                |
| `700`    | Жирный Bold                                                     |
| `800`    | Дополнительный жирный (Сверхжирный) Extra Bold (Ultra Bold)     |
| `900`    | Чёрный (Густой) Black (Heavy)                                   |

### Интерполяция

Значения `font-weight` интерполируются с помощью дискретных шагов (кратные 100). Интерполяция происходит в действительном пространстве чисел и превращается в целое число путём округления до ближайшего числа, кратного 100, со значениями посредине между кратными 100 округлёнными в сторону положительной бесконечности.

### Формальный синтаксис

{{csssyntax}}

## Примеры

### HTML

```html
<p>
  Alice was beginning to get very tired of sitting by her sister on the bank,
  and of having nothing to do: once or twice she had peeped into the book her
  sister was reading, but it had no pictures or conversations in it, 'and what
  is the use of a book,' thought Alice 'without pictures or conversations?'
</p>

<div>
  I'm heavy<br />
  <span>I'm lighter</span>
</div>
```

### CSS

```css
/* Назначение тексту элемента <p> жирного начертания. */
p {
  font-weight: bold;
}

/* Назначение тексту элемента <div> жирности, которая больше на два уровня,
чем normal, но все ещё меньше, чем стандартный bold. */
div {
  font-weight: 600;
}

/* Назначение тексту элемента <span> жирности,
которая на один уровень меньше, чем у его родителя. */
span {
  font-weight: lighter;
}
```

### Result

{{EmbedLiveSample("Примеры","400","300")}}

## Спецификации

{{Specifications}}

{{cssinfo}}

## Совместимость с браузерами

{{Compat}}
