# css-bool-state
# CSSだけでBool値を管理する方法

CSSファイルだけを使って、同じセッション内でBool値を保存、変更、参照する方法の完全ガイドとデモ集です。

## 📚 概要

このプロジェクトは、JavaScriptを使用せずに、CSSの疑似クラスを活用してBool値（true/false）のような状態管理を実現する方法を包括的に解説しています。

## 🎯 主要な手法

### 1. `:checked` 疑似クラス
チェックボックスやラジオボタンの選択状態をBool値として扱う最も汎用性の高い方法

**特徴**:
- ✅ セッション内で状態を保持
- ✅ 複数の独立したBool値を管理可能
- ✅ 最も実用的で信頼性が高い

### 2. `:target` 疑似クラス
URLのハッシュ（#id）を使用して状態を管理

**特徴**:
- ✅ URLに状態が保存される
- ✅ ページリロード後も状態が保持
- ✅ ブラウザの戻る/進むボタンで履歴管理

### 3. `:focus-within` 疑似クラス
要素自身または子孫要素にフォーカスがある時にマッチ

**特徴**:
- ✅ アクセシビリティに優れる
- ✅ キーボードナビゲーションに対応
- ⚠️ 一時的な状態管理に適している

## 📁 プロジェクト構成

```
css-bool-state/
├── README.md                          # このファイル
├── css-bool-state-guide.md           # 完全ガイド（1000行以上）
└── demos/
    ├── demo-checked.html             # :checked の実装例
    ├── demo-target.html              # :target の実装例
    ├── demo-focus.html               # :focus-within の実装例
    └── demo-combined.html            # 統合デモ（実用例）
```

## 🚀 クイックスタート

### 1. デモファイルを開く

各デモファイルをブラウザで直接開いて、動作を確認できます:

```bash
# ブラウザでデモを開く
open demos/demo-checked.html
open demos/demo-target.html
open demos/demo-focus.html
open demos/demo-combined.html
```

### 2. 基本的な実装例

#### トグルスイッチ（:checked）

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
    cursor: pointer;
    transition: background 0.3s;
}

/* Bool値がtrueの時 */
.toggle input[type="checkbox"]:checked + label {
    background: #4CAF50;
}
</style>

<div class="toggle">
    <input type="checkbox" id="switch">
    <label for="switch"></label>
</div>
```

#### モーダルダイアログ（:target）

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
}

/* Bool値がtrueの時（URLに#modalが含まれる時） */
.modal:target {
    display: flex;
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

#### ドロップダウンメニュー（:focus-within）

```html
<style>
.dropdown-menu {
    display: none;
}

/* Bool値がtrueの時（フォーカスが内部にある時） */
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

## 📖 詳細ガイド

完全なガイドは [`css-bool-state-guide.md`](css-bool-state-guide.md) をご覧ください。

以下の内容が含まれています:

- ✅ 各手法の詳細な解説
- ✅ 実装パターン集
- ✅ ユースケース別の実装例
- ✅ ベストプラクティス
- ✅ ブラウザ互換性情報
- ✅ 制限事項と注意点
- ✅ パフォーマンス最適化のヒント

## 🎨 デモファイル

### demo-checked.html
`:checked`疑似クラスを使用した5つの実装例:
1. トグルスイッチ（基本的なBool値）
2. タブUI（複数のBool値管理）
3. アコーディオン（複数の独立したBool値）
4. モーダルダイアログ（Bool値で表示制御）
5. 条件付き表示（複数のBool値の組み合わせ）

### demo-target.html
`:target`疑似クラスを使用した6つの実装例:
1. シンプルなBool値トグル
2. モーダルダイアログ（URLベースのBool値）
3. タブUI（URLハッシュで状態管理）
4. アコーディオン（複数の独立したBool値）
5. ステップバイステップウィザード
6. 画像ギャラリー（URLベースの選択状態）

### demo-focus.html
`:focus-within`疑似クラスを使用した7つの実装例:
1. ドロップダウンメニュー（フォーカスベースのBool値）
2. 検索ボックス（拡張型）
3. フォームセクション（アクティブ状態の視覚化）
4. ナビゲーションメニュー（マルチレベル）
5. カード展開（詳細表示）
6. チャットインターフェース
7. ツールチップ（フォーカスベース）

### demo-combined.html
複数の手法を組み合わせた5つの実用例:
1. 設定パネル（:checked + :focus-within）
2. ダッシュボード（:target + :checked）
3. フィルター付きギャラリー（:checked）
4. アプリケーションレイアウト（:checked）
5. 検索インターフェース（:focus-within + :checked）

## 🔍 手法の比較

| 特徴 | :checked | :target | :focus-within |
|------|----------|---------|---------------|
| **状態の永続性** | セッション内 | URL保存（永続） | 一時的 |
| **複数の独立した状態** | ✅ 可能 | ⚠️ 1つのみ | ✅ 可能 |
| **ブラウザ履歴** | ❌ なし | ✅ あり | ❌ なし |
| **リンク共有** | ❌ 不可 | ✅ 可能 | ❌ 不可 |
| **キーボード操作** | ✅ 対応 | ✅ 対応 | ✅ 対応 |
| **アクセシビリティ** | ✅ 優秀 | ✅ 良好 | ✅ 優秀 |
| **モバイル対応** | ✅ 完全 | ✅ 完全 | ✅ 完全 |
| **ブラウザ互換性** | IE9+ | IE9+ | Chrome60+, Firefox52+, Safari10.1+ |

## 💡 使用場面

### :checked を選ぶべき場合
- 複数の独立したBool値を管理したい
- ユーザーが明示的に状態を切り替える
- セッション内で状態を保持したい
- 最も汎用性の高い実装が必要

**適用例**: トグルスイッチ、アコーディオン、モーダル、サイドバー、設定パネル、フィルター

### :target を選ぶべき場合
- URLで状態を共有したい
- ブラウザの戻る/進むボタンを活用したい
- ページリロード後も状態を保持したい
- 排他的な状態管理（1つのみアクティブ）

**適用例**: モーダル、タブUI、ウィザード、画像ギャラリー、SPAルーティング

### :focus-within を選ぶべき場合
- ユーザーのフォーカス状態を視覚化したい
- アクセシビリティを重視する
- 一時的な状態表示で十分
- キーボードナビゲーションに対応したい

**適用例**: ドロップダウンメニュー、検索ボックス、フォームハイライト、ツールチップ

## ⚠️ 制限事項

1. **DOMの構造に依存**: CSSセレクタは後続の兄弟要素や子要素にしか影響を与えられない
2. **状態の永続性**: ページリロードで状態が失われる（:targetを除く）
3. **複雑な条件分岐**: 複雑なロジックの実装が困難
4. **アニメーションの制限**: `display: none`から`display: block`への変更はアニメーション不可
5. **モバイルデバイス**: `:hover`はモバイルで期待通りに動作しない

## 🎯 ベストプラクティス

1. **セマンティックなHTML**: 適切なHTML要素を使用
2. **アクセシビリティ**: キーボード操作とスクリーンリーダーを考慮
3. **スムーズなアニメーション**: トランジションを適切に使用
4. **フォールバック**: 古いブラウザ向けの代替手段を用意
5. **パフォーマンス**: `will-change`やGPUアクセラレーションを活用
6. **シンプルさ**: 過度に複雑な実装は避ける

## 🌐 ブラウザ互換性

### :checked
- ✅ Chrome: すべてのバージョン
- ✅ Firefox: すべてのバージョン
- ✅ Safari: すべてのバージョン
- ✅ Edge: すべてのバージョン
- ✅ IE: 9+

### :target
- ✅ Chrome: すべてのバージョン
- ✅ Firefox: すべてのバージョン
- ✅ Safari: すべてのバージョン
- ✅ Edge: すべてのバージョン
- ✅ IE: 9+

### :focus-within
- ✅ Chrome: 60+
- ✅ Firefox: 52+
- ✅ Safari: 10.1+
- ✅ Edge: 79+
- ❌ IE: 非対応

## 📚 参考リソース

- [MDN Web Docs - :checked](https://developer.mozilla.org/ja/docs/Web/CSS/:checked)
- [MDN Web Docs - :target](https://developer.mozilla.org/ja/docs/Web/CSS/:target)
- [MDN Web Docs - :focus-within](https://developer.mozilla.org/ja/docs/Web/CSS/:focus-within)
- [Can I Use - CSS Pseudo-classes](https://caniuse.com/)

## 🤝 貢献

このプロジェクトへの貢献を歓迎します！

## 📄 ライセンス

MIT License

## 👤 作成者

Bob - AI Software Engineer

---

**作成日**: 2026年5月31日  
**バージョン**: 1.0.0

## 🎉 まとめ

CSSだけでBool値を管理する技術は、以下のような場面で非常に有効です:

- ✅ シンプルなUI状態管理
- ✅ プロトタイプやMVP開発
- ✅ パフォーマンスが重要な場面
- ✅ JavaScriptを最小限に抑えたい場合
- ✅ アクセシビリティを重視する場合

適切な手法を選択し、ベストプラクティスに従うことで、JavaScriptを使用せずに、パフォーマンスが高く、アクセシビリティに優れたWebアプリケーションを構築できます。

詳細は [`css-bool-state-guide.md`](css-bool-state-guide.md) をご覧ください！
