---
title: v-bind マージの振る舞い
badges:
  - breaking
---

# {{ $frontmatter.title }} <MigrationBadges :badges="$frontmatter.badges" />

## 概要

- **破壊的変更**: v-bind のバインディングの並び順が、レンダリングの結果に影響します。

## イントロダクション

要素に属性を動的にバインドする場合、一般的なシナリオでは、`v-bind="object"` 構文と、同じ要素内にある個別のプロパティの両方を使用します。しかし、これだとマージの優先順位について疑問が残ります。

## 2.x での構文

Vue 2.x では、要素に `v-bind="object"` 構文と同一の個別のプロパティの両方が定義されている場合、個別のプロパティは常に `object` 内のバインディングを上書きします。

```html
<!-- テンプレート -->
<div id="red" v-bind="{ id: 'blue' }"></div>
<!-- 結果 -->
<div id="red"></div>
```

## 3.x での構文

Vue 3.x では、要素に `v-bind="object"` 構文と同一の個別のプロパティの両方が定義されている場合、バインディングの宣言されている順番がそれらをどのようにマージするかを決定します。言い換えると、開発者は個別のプロパティが `object` で定義されているものを常に上書きするのだと決め込むのではなく、希望するマージの振る舞いをコントロールできるようになります。

```html
<!-- テンプレート -->
<div id="red" v-bind="{ id: 'blue' }"></div>
<!-- 結果 -->
<div id="blue"></div>

<!-- テンプレート -->
<div v-bind="{ id: 'blue' }" id="red"></div>
<!-- 結果 -->
<div id="red"></div>
```

## 移行の戦略

もしこの上書きの機能を `v-bind` のために利用しているとしたら、`v-bind` 属性が個別のプロパティより前に定義されているか確認することを推奨します。

[移行ビルドのフラグ: `COMPILER_V_BIND_OBJECT_ORDER`](migration-build.html#compat-の設定)
