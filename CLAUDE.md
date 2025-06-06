# Slidevプロジェクト構成ガイド

## プロジェクト構造
```
slidevテスト/
├── package.json          # Slidev依存関係とスクリプト
├── node_modules/         # インストールされたパッケージ
├── slidevの使い方/
│   └── slides.md        # メインのスライドファイル
└── slides-export.pdf    # エクスポートされたPDF
```

## 初期セットアップ

### 1. package.jsonの作成
```json
{
  "name": "slidev-test",
  "type": "module",
  "private": true,
  "scripts": {
    "dev": "slidev slidevの使い方/slides.md --open",
    "build": "slidev build slidevの使い方/slides.md",
    "export": "slidev export slidevの使い方/slides.md"
  },
  "dependencies": {
    "@slidev/cli": "^0.49.0",
    "@slidev/theme-default": "latest"
  },
  "devDependencies": {
    "playwright-chromium": "^1.52.0"
  }
}
```

### 2. 依存関係のインストール
```bash
npm install
npm i -D playwright-chromium  # エクスポート機能に必要
```

## よくあるエラーと対処法

### 1. 文字化け問題
- **原因**: ファイルのエンコーディングがUTF-8でない
- **対処法**: ファイルをUTF-8で保存し直す

### 2. PDFエクスポート時に1ページ目しか出力されない
- **原因**: Slidevプロジェクトとして正しく初期化されていない
- **対処法**: 
  - package.jsonを作成
  - 依存関係をインストール
  - スライドファイルのパスを正しく指定

### 3. `<v-clicks>`タグがそのまま表示される
- **原因**: 
  - Slidevが正しくインストールされていない
  - 開発サーバーではなく静的ファイルを直接開いている
- **対処法**:
  - `npm run dev`で開発サーバーを起動
  - `npm run build`でHTMLビルド
  - `npm run export`でPDF/PPTXエクスポート

### 4. エクスポート時にコンテンツがページからはみ出る
- **対処法**:
  ```yaml
  ---
  aspectRatio: '16/9'
  canvasWidth: 980
  ---
  ```
  - 1スライドの情報量を減らす
  - レイアウトを調整

## スライドの基本構造

### フロントマター（最初のスライド）
```yaml
---
theme: default
aspectRatio: '16/9'
canvasWidth: 980
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  ## スライドのタイトル
  説明文
drawings:
  persist: false
transition: slide-left
title: スライドタイトル
---
```

### スライドの区切り
- `---` で新しいスライドを作成
- `--` でアニメーション付きの区切り（クリックで次の要素）

### レイアウトオプション
```yaml
---
layout: center  # center, cover, intro, section, quote, fact, statement
class: text-center
---
```

## アニメーション機能

### v-clickの使い方（推奨）
```markdown
<div v-click>

- 最初のアイテム

</div>
<div v-click>

- 2番目のアイテム

</div>
```

### v-clicksの使い方（動作しない場合がある）
```markdown
<v-clicks>

- アイテム1
- アイテム2
- アイテム3

</v-clicks>
```

## コードハイライト
```markdown
```js {2,3}
function hello() {
  console.log('Hello')  // ハイライト
  console.log('World')  // ハイライト
}
```
```

## 便利なコマンド

### 開発
```bash
npm run dev              # 開発サーバー起動（ホットリロード付き）
```

### ビルド・エクスポート
```bash
npm run build            # HTMLファイルとしてビルド
npm run export           # PDFとしてエクスポート
npm run export -- --format png   # PNG画像として各スライドをエクスポート
npm run export -- --format pptx  # PowerPointファイルとしてエクスポート
```

### キーボードショートカット（プレゼン中）
- `Space` / `→`: 次のスライド
- `←`: 前のスライド
- `f`: フルスクリーン
- `o`: スライド一覧
- `d`: ダークモード切り替え
- `p`: プレゼンターモード
- `r`: 録画開始/停止

## プレゼンターノート
```markdown
---
# スライドタイトル

スライドの内容

<!--
ここにプレゼンターノートを書く
プレゼンターモードでのみ表示される
-->
```

## 画像・アセット
- 画像は`public/`フォルダに配置
- スライド内では`/image.png`のように参照

## トラブルシューティング

1. **スライドが表示されない**
   - `npm run dev`でサーバーが起動しているか確認
   - ブラウザで`http://localhost:3030`にアクセス

2. **エクスポートが失敗する**
   - `playwright-chromium`がインストールされているか確認
   - `npm i -D playwright-chromium`を実行

3. **日本語が文字化けする**
   - ファイルのエンコーディングをUTF-8に設定
   - エディタの設定を確認

## 今後の改善点
- Mermaidダイアグラムの活用
- カスタムテーマの作成
- Vueコンポーネントの組み込み
- LaTeX数式の活用