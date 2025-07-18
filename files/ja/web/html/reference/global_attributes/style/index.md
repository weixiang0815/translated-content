---
title: style
slug: Web/HTML/Reference/Global_attributes/style
original_slug: Web/HTML/Global_attributes/style
l10n:
  sourceCommit: ba96f2f183353872db6d9242c7d2dffe2dbc0c35
---

{{HTMLSidebar("Global_attributes")}}

**`style`** [グローバル属性](/ja/docs/Web/HTML/Reference/Global_attributes)は、要素に適用する [CSS](/ja/docs/Web/CSS) スタイル宣言を指定します。なお、スタイルは別のファイルで定義することが推奨されます。この属性と {{HTMLElement("style")}} 要素の主な用途は、例えばテストのために、すばやくスタイルを適用できるようにすることです。

{{InteractiveExample("HTML デモ: style", "tabbed-shorter")}}

```html interactive-example
<div style="background: #ffe7e8; border: 2px solid #e66465">
  <p style="margin: 15px; line-height: 1.5; text-align: center">
    Well, I am the slime from your video<br />
    Oozin' along on your livin' room floor.
  </p>
</div>
```

> [!NOTE]
> この属性は、意味的な情報を伝えるために使用してはいけません。すべてのスタイルが削除されたとしても、ページは依然として意味的にが正しくあるべきです。特に、無関係な情報を隠すために使用しないようにしてください。これには [`hidden`](/ja/docs/Web/HTML/Reference/Global_attributes/hidden) 属性を使用してください。

## 仕様書

{{Specifications}}

## ブラウザーの互換性

{{Compat}}

## 関連情報

- すべての[グローバル属性](/ja/docs/Web/HTML/Reference/Global_attributes)
