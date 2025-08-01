---
title: scroll-behavior
slug: Web/CSS/scroll-behavior
l10n:
  sourceCommit: 215e1b1590b1210152e3570627933c0303171e11
---

**`scroll-behavior`** は [CSS](/ja/docs/Web/CSS) のプロパティで、ナビゲーションや CSSOM のスクローリング API によってスクロールするボックスにスクロールが発生した際の、そのスクロールの振る舞いを設定します。

{{InteractiveExample("CSS デモ: scroll-behavior")}}

```css interactive-example-choice
scroll-behavior: auto;
```

```css interactive-example-choice
scroll-behavior: smooth;
```

```html interactive-example
<section id="default-example">
  <div class="container">
    <p class="nav">
      スクロール先:
      <a href="#pageA">A</a>
      <a href="#pageB">B</a>
      <a href="#pageC">C</a>
    </p>
    <scroll-container id="example-element">
      <scroll-page id="pageA">A</scroll-page>
      <scroll-page id="pageB">B</scroll-page>
      <scroll-page id="pageC">C</scroll-page>
    </scroll-container>
  </div>
</section>
```

```css interactive-example
.container {
  flex-direction: column;
}

.nav a {
  color: #009e5f;
}

scroll-container {
  border: 1px solid black;
  display: block;
  height: 200px;
  overflow-y: scroll;
  width: 200px;
}

scroll-page {
  align-items: center;
  display: flex;
  font-size: 5em;
  height: 100%;
  justify-content: center;
}
```

なお、ユーザーが実行したスクロールなど、その他のスクロールはこのプロパティの影響を受けません。このプロパティがルート要素に指定された場合は、代わりにビューポートに適用されます。このプロパティが `body` 要素に指定された場合は、ビューポートには適用され*ません*。

ユーザーエージェントは、このプロパティを無視することができます。

## 構文

```css
/* キーワード値 */
scroll-behavior: auto;
scroll-behavior: smooth;

/* グローバル値 */
scroll-behavior: inherit;
scroll-behavior: initial;
scroll-behavior: revert;
scroll-behavior: revert-layer;
scroll-behavior: unset;
```

`scroll-behavior` プロパティは、以下の一覧にあるキーワード値のうちの一つで指定します。

### 値

- `auto`
  - : スクロールするボックスは瞬時にスクロールします。
- `smooth`
  - : スクロールするボックスは、ユーザーエージェント定義のイージング関数を使い、ユーザーエージェント定義の時間をかけて、円滑にスクロールします。もし存在するなら、ユーザーエージェントはプラットフォームの慣例に従うべきです。

## 公式定義

{{cssinfo}}

## 形式文法

{{csssyntax}}

## 例

### スクロールの動きをスムーズに設定

#### HTML

```html
<nav>
  <a href="#page-1">1</a>
  <a href="#page-2">2</a>
  <a href="#page-3">3</a>
</nav>
<div class="scroll-container">
  <div class="scroll-page" id="page-1">1</div>
  <div class="scroll-page" id="page-2">2</div>
  <div class="scroll-page" id="page-3">3</div>
</div>
```

#### CSS

```css
a {
  display: inline-block;
  width: 50px;
  text-decoration: none;
}
nav,
.scroll-container {
  display: block;
  margin: 0 auto;
  text-align: center;
}
nav {
  width: 339px;
  padding: 5px;
  border: 1px solid black;
}
.scroll-container {
  width: 350px;
  height: 200px;
  overflow-y: scroll;
  scroll-behavior: smooth;
}
.scroll-page {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100%;
  font-size: 5em;
}
```

#### 結果

{{ EmbedLiveSample("スクロールの動きをスムーズに設定", "100%", 250) }}

## 仕様書

{{Specifications}}

## ブラウザーの互換性

{{Compat}}
