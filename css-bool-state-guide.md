# CSSだけでBool値を管理する完全ガイド

## 📋 目次

1. [概要](#概要)
2. [主要な手法](#主要な手法)
3. [手法の比較](#手法の比較)
4. [実装パターン](#実装パターン)
5. [ユースケース](#ユースケース)
6. [ベストプラクティス](#ベストプラクティス)
7. [ブラウザ互換性](#ブラウザ互換性)
8. [制限事項と注意点](#制限事項と注意点)

---

## 概要

CSSには変数や状態を直接保存する機能はありませんが、特定の疑似クラスを活用することで、Bool値（true/false）のような状態管理を実現できます。この技術により、JavaScriptを使用せずに、インタラクティブなUIコンポーネントを構築できます。

### なぜCSSでBool値管理が重要なのか？

- **パフォーマンス**: JavaScriptの実行オーバーヘッドがない
- **シンプルさ**: 追加のスクリプトが不要
- **保守性**: CSSだけで完結するため、コードの見通しが良い
- **アクセシビリティ**: ネイティブなHTML要素を活用するため、スクリーンリーダーなどに対応しやすい
- **軽量**: JavaScriptライブラリが不要

---

## 主要な手法

### 1. `:checked` 疑似クラス

**概要**: チェックボックスやラジオボタンの選択状態をBool値として扱う

**特徴**:
- ✅ セッション内で状態を保持
- ✅ 複数の独立したBool値を管理可能
- ✅ ユーザーが明示的に変更可能
- ✅ 最も汎用性が高い

**基本構文**:
```css
/* チェックボックスを非表示にする */
input[type="checkbox"] {
    display: none;
}

/* Bool値がtrueの時（チェックされている時） */
input[type="checkbox"]:checked + label {
    background: green;
}

/* Bool値を参照して他の要素を制御 */
input[type="checkbox"]:checked ~ .content {
    display: block;
}
```

**実装例**:
```html
<!-- トグルスイッチ -->
<input type="checkbox" id="toggle">
<label for="toggle">ON/OFF</label>
<div class="content">表示されるコンテンツ</div>
```

**適用場面**:
- トグルスイッチ
- アコーディオン（複数同時展開可能）
- モーダルダイアログ
- サイドバーの表示/非表示
- 設定パネル
- フィルター機能

---

### 2. `:target` 疑似クラス

**概要**: URLのハッシュ（#id）を使用して状態を管理

**特徴**:
- ✅ URLに状態が保存される
- ✅ ページリロード後も状態が保持
- ✅ ブラウザの戻る/進むボタンで履歴管理
- ✅ ブックマークやリンク共有で特定の状態を共有可能
- ⚠️ 一度に1つの要素のみターゲット可能

**基本構文**:
```css
/* デフォルト状態（非表示） */
.modal {
    display: none;
}

/* Bool値がtrueの時（URLに#modalが含まれる時） */
.modal:target {
    display: block;
}
```

**実装例**:
```html
<!-- モーダルを開くリンク -->
<a href="#modal">モーダルを開く</a>

<!-- モーダル -->
<div id="modal" class="modal">
    <div class="modal-content">
        <h2>モーダルタイトル</h2>
        <a href="#">閉じる</a>
    </div>
</div>
```

**適用場面**:
- モーダルダイアログ
- タブUI（排他的な選択）
- アコーディオン（1つのみ展開）
- ステップバイステップウィザード
- 画像ギャラリー
- シングルページアプリケーションのルーティング

---

### 3. `:focus-within` 疑似クラス

**概要**: 要素自身または子孫要素にフォーカスがある時にマッチ

**特徴**:
- ✅ アクセシビリティに優れる
- ✅ キーボードナビゲーションに対応
- ✅ ユーザーインタラクション中のみ有効
- ⚠️ フォーカスが外れると状態が失われる（一時的なBool値）
- ⚠️ フォーカス可能な要素が必要

**基本構文**:
```css
/* デフォルト状態 */
.dropdown-menu {
    display: none;
}

/* Bool値がtrueの時（フォーカスが内部にある時） */
.dropdown:focus-within .dropdown-menu {
    display: block;
}
```

**実装例**:
```html
<!-- ドロップダウンメニュー -->
<div class="dropdown">
    <button>メニュー</button>
    <div class="dropdown-menu">
        <a href="#">オプション 1</a>
        <a href="#">オプション 2</a>
    </div>
</div>
```

**適用場面**:
- ドロップダウンメニュー
- 検索ボックスの拡張と候補表示
- フォームセクションのハイライト
- ツールチップ
- ナビゲーションメニュー
- チャット入力エリアのアクティブ状態

---

### 4. その他の手法

#### `:hover` 疑似クラス

**特徴**:
- マウスホバー中のみ有効
- 一時的な状態表示に適している
- モバイルデバイスでは動作が異なる

```css
.card:hover .details {
    display: block;
}
```

#### CSS カスタムプロパティ（変数）

**特徴**:
- 値の保存と参照が可能
- 他の手法と組み合わせて使用
- Bool値そのものではなく、状態に応じた値を保存

```css
:root {
    --is-dark-mode: 0;
}

input[type="checkbox"]:checked {
    --is-dark-mode: 1;
}

.element {
    opacity: var(--is-dark-mode);
}
```

---

## 手法の比較

| 特徴 | :checked | :target | :focus-within | :hover |
|------|----------|---------|---------------|--------|
| **状態の永続性** | セッション内 | URL保存（永続） | 一時的 | 一時的 |
| **複数の独立した状態** | ✅ 可能 | ⚠️ 1つのみ | ✅ 可能 | ✅ 可能 |
| **ブラウザ履歴** | ❌ なし | ✅ あり | ❌ なし | ❌ なし |
| **リンク共有** | ❌ 不可 | ✅ 可能 | ❌ 不可 | ❌ 不可 |
| **キーボード操作** | ✅ 対応 | ✅ 対応 | ✅ 対応 | ⚠️ 限定的 |
| **アクセシビリティ** | ✅ 優秀 | ✅ 良好 | ✅ 優秀 | ⚠️ 注意必要 |
| **モバイル対応** | ✅ 完全 | ✅ 完全 | ✅ 完全 | ⚠️ 制限あり |
| **実装の複雑さ** | 低 | 低 | 低 | 低 |
| **ブラウザ互換性** | ✅ 優秀 | ✅ 優秀 | ✅ 良好 | ✅ 優秀 |

### 選択ガイド

**`:checked` を選ぶべき場合**:
- 複数の独立したBool値を管理したい
- ユーザーが明示的に状態を切り替える
- セッション内で状態を保持したい
- 最も汎用性の高い実装が必要

**`:target` を選ぶべき場合**:
- URLで状態を共有したい
- ブラウザの戻る/進むボタンを活用したい
- ページリロード後も状態を保持したい
- 排他的な状態管理（1つのみアクティブ）

**`:focus-within` を選ぶべき場合**:
- ユーザーのフォーカス状態を視覚化したい
- アクセシビリティを重視する
- 一時的な状態表示で十分
- キーボードナビゲーションに対応したい

---

## 実装パターン

### パターン1: トグルスイッチ

```html
<style>
.toggle input[type="checkbox"] {
    display: none;
}

.toggle label {
    display: inline-block;
    width: 60px;
    height: 30px;
    background: #ccc;
    border-radius: 15px;
    position: relative;
    cursor: pointer;
    transition: background 0.3s;
}

.toggle label::after {
    content: '';
    position: absolute;
    width: 24px;
    height: 24px;
    background: white;
    border-radius: 50%;
    top: 3px;
    left: 3px;
    transition: transform 0.3s;
}

.toggle input[type="checkbox"]:checked + label {
    background: #4CAF50;
}

.toggle input[type="checkbox"]:checked + label::after {
    transform: translateX(30px);
}
</style>

<div class="toggle">
    <input type="checkbox" id="switch">
    <label for="switch"></label>
</div>
```

### パターン2: タブUI

```html
<style>
.tabs input[type="radio"] {
    display: none;
}

.tab-labels {
    display: flex;
}

.tab-labels label {
    flex: 1;
    padding: 15px;
    cursor: pointer;
    background: #f0f0f0;
}

.tabs input[type="radio"]:checked + label {
    background: #667eea;
    color: white;
}

.tab-content {
    display: none;
    padding: 20px;
}

#tab1:checked ~ .tab-contents #content1,
#tab2:checked ~ .tab-contents #content2 {
    display: block;
}
</style>

<div class="tabs">
    <input type="radio" name="tabs" id="tab1" checked>
    <input type="radio" name="tabs" id="tab2">
    
    <div class="tab-labels">
        <label for="tab1">タブ1</label>
        <label for="tab2">タブ2</label>
    </div>
    
    <div class="tab-contents">
        <div class="tab-content" id="content1">コンテンツ1</div>
        <div class="tab-content" id="content2">コンテンツ2</div>
    </div>
</div>
```

### パターン3: モーダルダイアログ（:checked版）

```html
<style>
.modal-trigger input[type="checkbox"] {
    display: none;
}

.modal-overlay {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.7);
    z-index: 1000;
}

.modal-content {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: white;
    padding: 40px;
    border-radius: 15px;
}

.modal-trigger input[type="checkbox"]:checked ~ .modal-overlay {
    display: block;
}
</style>

<div class="modal-trigger">
    <input type="checkbox" id="modal">
    <label for="modal">モーダルを開く</label>
    
    <div class="modal-overlay">
        <div class="modal-content">
            <h2>モーダル</h2>
            <label for="modal">閉じる</label>
        </div>
    </div>
</div>
```

### パターン4: モーダルダイアログ（:target版）

```html
<style>
.modal {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.7);
    z-index: 1000;
}

.modal:target {
    display: flex;
    align-items: center;
    justify-content: center;
}

.modal-content {
    background: white;
    padding: 40px;
    border-radius: 15px;
}
</style>

<a href="#modal">モーダルを開く</a>

<div id="modal" class="modal">
    <div class="modal-content">
        <h2>モーダル</h2>
        <a href="#">閉じる</a>
    </div>
</div>
```

### パターン5: アコーディオン

```html
<style>
.accordion-item input[type="checkbox"] {
    display: none;
}

.accordion-header {
    padding: 15px;
    background: #667eea;
    color: white;
    cursor: pointer;
}

.accordion-content {
    max-height: 0;
    overflow: hidden;
    transition: max-height 0.3s;
}

.accordion-item input[type="checkbox"]:checked ~ .accordion-content {
    max-height: 500px;
}
</style>

<div class="accordion-item">
    <input type="checkbox" id="acc1">
    <label class="accordion-header" for="acc1">セクション1</label>
    <div class="accordion-content">
        <p>コンテンツ</p>
    </div>
</div>
```

### パターン6: ドロップダウンメニュー

```html
<style>
.dropdown {
    position: relative;
}

.dropdown-menu {
    position: absolute;
    display: none;
    background: white;
    box-shadow: 0 5px 15px rgba(0,0,0,0.2);
}

.dropdown:focus-within .dropdown-menu {
    display: block;
}
</style>

<div class="dropdown">
    <button>メニュー</button>
    <div class="dropdown-menu">
        <a href="#">オプション1</a>
        <a href="#">オプション2</a>
    </div>
</div>
```

### パターン7: 複数Bool値のAND条件

```html
<style>
/* すべての条件がtrueの時のみ表示 */
#cond1:checked ~ #cond2:checked ~ #cond3:checked ~ .result {
    display: block;
}
</style>

<input type="checkbox" id="cond1">
<label for="cond1">条件1</label>

<input type="checkbox" id="cond2">
<label for="cond2">条件2</label>

<input type="checkbox" id="cond3">
<label for="cond3">条件3</label>

<div class="result">すべての条件が満たされました</div>
```

---

## ユースケース

### 1. ダークモード切り替え

```css
body {
    --bg-color: white;
    --text-color: black;
    background: var(--bg-color);
    color: var(--text-color);
}

#dark-mode:checked ~ body {
    --bg-color: #1a1a1a;
    --text-color: white;
}
```

### 2. サイドバーの表示/非表示

```css
.sidebar {
    width: 250px;
    transition: width 0.3s;
}

#sidebar-toggle:not(:checked) ~ .layout .sidebar {
    width: 0;
    overflow: hidden;
}
```

### 3. フィルター機能

```css
/* デフォルトですべて表示 */
.item {
    display: block;
}

/* フィルターがアクティブの時 */
#filter-active:checked ~ .list .item:not(.active) {
    display: none;
}
```

### 4. ステップバイステップフォーム

```css
.step {
    display: none;
}

#step1:target,
#step2:target,
#step3:target {
    display: block;
}
```

### 5. 画像ギャラリー

```css
.gallery-image {
    display: none;
}

.gallery-image:target {
    display: block;
}
```

---

## ベストプラクティス

### 1. セマンティックなHTML

```html
<!-- 良い例 -->
<input type="checkbox" id="menu-toggle" aria-label="メニューを開く">
<label for="menu-toggle">メニュー</label>

<!-- 悪い例 -->
<div onclick="toggleMenu()">メニュー</div>
```

### 2. アクセシビリティの考慮

```css
/* フォーカス状態を明示 */
input:focus + label {
    outline: 2px solid blue;
    outline-offset: 2px;
}

/* スクリーンリーダー用のテキスト */
.sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    overflow: hidden;
    clip: rect(0,0,0,0);
}
```

### 3. スムーズなアニメーション

```css
/* トランジションを使用 */
.element {
    transition: all 0.3s ease-in-out;
}

/* アニメーションを使用 */
@keyframes fadeIn {
    from { opacity: 0; }
    to { opacity: 1; }
}

.element:target {
    animation: fadeIn 0.3s;
}
```

### 4. フォールバック

```css
/* 古いブラウザ向けのフォールバック */
.dropdown-menu {
    display: none;
}

/* モダンブラウザ */
@supports (selector(:focus-within)) {
    .dropdown:focus-within .dropdown-menu {
        display: block;
    }
}
```

### 5. パフォーマンスの最適化

```css
/* will-changeを使用してパフォーマンスを向上 */
.modal {
    will-change: transform, opacity;
}

/* GPUアクセラレーションを活用 */
.element {
    transform: translateZ(0);
}
```

### 6. 適切なセレクタの使用

```css
/* 良い例: 隣接セレクタ */
input:checked + label {
    background: green;
}

/* 良い例: 一般兄弟セレクタ */
input:checked ~ .content {
    display: block;
}

/* 避けるべき: 過度に複雑なセレクタ */
input:checked ~ div > ul > li:nth-child(2) .content {
    display: block;
}
```

---

## ブラウザ互換性

### `:checked` 疑似クラス
- ✅ Chrome: すべてのバージョン
- ✅ Firefox: すべてのバージョン
- ✅ Safari: すべてのバージョン
- ✅ Edge: すべてのバージョン
- ✅ IE: 9+

### `:target` 疑似クラス
- ✅ Chrome: すべてのバージョン
- ✅ Firefox: すべてのバージョン
- ✅ Safari: すべてのバージョン
- ✅ Edge: すべてのバージョン
- ✅ IE: 9+

### `:focus-within` 疑似クラス
- ✅ Chrome: 60+
- ✅ Firefox: 52+
- ✅ Safari: 10.1+
- ✅ Edge: 79+
- ❌ IE: 非対応

### 互換性対策

```css
/* :focus-withinのポリフィル的アプローチ */
.dropdown button:focus + .dropdown-menu,
.dropdown .dropdown-menu:hover {
    display: block;
}

/* モダンブラウザ */
@supports (selector(:focus-within)) {
    .dropdown:focus-within .dropdown-menu {
        display: block;
    }
}
```

---

## 制限事項と注意点

### 1. DOMの構造に依存

**問題**: CSSセレクタは後続の兄弟要素や子要素にしか影響を与えられない

```css
/* ✅ 可能: 後続の兄弟要素 */
input:checked ~ .content {
    display: block;
}

/* ❌ 不可能: 前の兄弟要素 */
.content:has(~ input:checked) {
    display: block;
}

/* ⚠️ 制限あり: 親要素（:has()が必要） */
.parent:has(input:checked) {
    background: green;
}
```

**解決策**: HTML構造を適切に設計する

```html
<!-- 良い構造 -->
<input type="checkbox" id="toggle">
<label for="toggle">トグル</label>
<div class="content">コンテンツ</div>

<!-- 悪い構造 -->
<div class="content">コンテンツ</div>
<input type="checkbox" id="toggle">
<label for="toggle">トグル</label>
```

### 2. 状態の永続性

**問題**: ページをリロードすると状態が失われる（:targetを除く）

**解決策**:
- `:target`を使用してURLに状態を保存
- LocalStorageとJavaScriptを組み合わせる（CSSのみでは不可）
- サーバーサイドで状態を管理

### 3. 複雑な条件分岐

**問題**: 複雑なロジックの実装が困難

```css
/* 複雑なAND/OR条件は冗長になる */
#a:checked ~ #b:checked ~ .result,
#a:checked ~ #c:checked ~ .result,
#b:checked ~ #c:checked ~ .result {
    display: block;
}
```

**解決策**: 
- シンプルな条件に分解
- 必要に応じてJavaScriptを使用

### 4. アニメーションの制限

**問題**: `display: none`から`display: block`への変更はアニメーション不可

**解決策**: `opacity`や`max-height`を使用

```css
/* ❌ アニメーション不可 */
.element {
    display: none;
    transition: display 0.3s;
}

.element:target {
    display: block;
}

/* ✅ アニメーション可能 */
.element {
    opacity: 0;
    max-height: 0;
    overflow: hidden;
    transition: opacity 0.3s, max-height 0.3s;
}

.element:target {
    opacity: 1;
    max-height: 1000px;
}
```

### 5. モバイルデバイスでの制限

**問題**: `:hover`はモバイルで期待通りに動作しない

**解決策**: `:focus-within`や`:checked`を使用

```css
/* モバイル非対応 */
.menu:hover .submenu {
    display: block;
}

/* モバイル対応 */
.menu:focus-within .submenu {
    display: block;
}
```

### 6. SEOへの影響

**問題**: 非表示コンテンツがSEOに影響する可能性

**解決策**:
- 重要なコンテンツはデフォルトで表示
- `display: none`の代わりに`visibility: hidden`や`opacity: 0`を検討
- サーバーサイドレンダリングを活用

---

## まとめ

CSSだけでBool値を管理する技術は、以下のような場面で非常に有効です:

### 使用を推奨する場面
- ✅ シンプルなUI状態管理
- ✅ プロトタイプやMVP開発
- ✅ パフォーマンスが重要な場面
- ✅ JavaScriptを最小限に抑えたい場合
- ✅ アクセシビリティを重視する場合

### JavaScriptを検討すべき場面
- ⚠️ 複雑な状態管理が必要
- ⚠️ 状態の永続化が必須
- ⚠️ 動的なコンテンツ生成
- ⚠️ 複雑なアニメーション
- ⚠️ APIとの連携

### 重要なポイント
1. **適切な手法を選択**: ユースケースに応じて`:checked`、`:target`、`:focus-within`を使い分ける
2. **アクセシビリティを優先**: セマンティックなHTMLとキーボード操作を考慮
3. **シンプルに保つ**: 過度に複雑な実装は避け、必要に応じてJavaScriptを使用
4. **ブラウザ互換性を確認**: 古いブラウザのサポートが必要な場合はフォールバックを用意
5. **パフォーマンスを意識**: トランジションとアニメーションを適切に使用

---

## デモファイル

このガイドに付属するデモファイルで、実際の動作を確認できます:

1. **demo-checked.html** - `:checked`疑似クラスの実装例
2. **demo-target.html** - `:target`疑似クラスの実装例
3. **demo-focus.html** - `:focus-within`疑似クラスの実装例
4. **demo-combined.html** - 複数手法を組み合わせた実用例

---

## 参考リソース

- [MDN Web Docs - :checked](https://developer.mozilla.org/ja/docs/Web/CSS/:checked)
- [MDN Web Docs - :target](https://developer.mozilla.org/ja/docs/Web/CSS/:target)
- [MDN Web Docs - :focus-within](https://developer.mozilla.org/ja/docs/Web/CSS/:focus-within)
- [Can I Use - CSS Pseudo-classes](https://caniuse.com/)

---

**作成日**: 2026年5月31日  
**バージョン**: 1.0.0  
**ライセンス**: MIT