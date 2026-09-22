# Zonebill LP（需要検証用ランディングページ）

ZoneLog（Zonebill）の実装前に需要を検証するための、コード不要で公開できる
最小限のランディングページ。日本語版・英語版は別ファイルで、反応を個別に計測できる。

- `index_ja.html` — 日本語版
- `index_en.html` — 英語版

## 公開前にやること

### 1. Formspreeのフォームを作成し、エンドポイントを埋め込む
1. [formspree.io](https://formspree.io) で無料アカウントを作成し、新規フォームを作成する
2. 発行された `https://formspree.io/f/xxxxxxxx` というURLを控える
3. 両ファイル内の `action="https://formspree.io/f/YOUR_FORM_ID"` を、実際のURLに置き換える（各ファイルに1箇所ずつ）
4. Formspreeの管理画面で、送信データに `locale`（`ja`/`en`）と `source`（流入元）の列が記録されることを確認する

### 2. GitHub Pagesで公開する
一番手軽な方法は、このリポジトリとは別に軽量な公開用リポジトリを1つ用意する方法（本体アプリのコードと混在させたくないため）。

1. GitHub上に新規リポジトリを作成（例: `zonebill-lp`）
2. `index_ja.html`・`index_en.html` をそのリポジトリ直下にコピー
3. **英語版をルートの `index.html` にリネーム**する（米国ターゲットが主軸のため。GitHub Pagesはルートの`index.html`をデフォルトで表示する）
   - 例: `index_en.html` → `index.html`
   - 日本語版は `ja/index.html` のようなサブパスに置くと、`https://<username>.github.io/zonebill-lp/` （英語）と `https://<username>.github.io/zonebill-lp/ja/` （日本語）のように、URL単位で日英の反応を分離して計測できる
4. リポジトリの Settings → Pages → Source で公開ブランチ（`main`など）を指定して有効化
5. 数分後に上記URLでページが公開される

### 3. Google Analyticsを有効化する（任意）
各ファイル末尾にコメントアウトされたGAスニペットがある。GA4の測定ID（`G-XXXXXXXXXX`）を取得し、2箇所ある `G-XXXXXXXXXX` を置き換えてコメントを外すと、ページ訪問の計測ができる。

## 流入元（コミュニティ）の計測方法
リンクを共有する際、URLの末尾に `?src=コミュニティ名` を付けるだけで、その値がフォーム送信時に自動的に隠しフィールド（`source`）へ入り、Formspree側の受信データで流入元を区別できる。

例:
```
https://<username>.github.io/zonebill-lp/?src=reddit_handyman
https://<username>.github.io/zonebill-lp/ja/?src=note_zeimu
```
`src`が付いていない場合は `direct` として記録される。

## 文言の差し替え方
両ファイルとも、差し替え対象のテキストには `data-i18n="キー名"` を付与してある（`placeholder`のみ `data-i18n-placeholder`）。文言確定後は、該当する `data-i18n` を目印にテキストを直接書き換えるだけでよい（JSによる動的差し替えは行っていない、プレーンな静的HTMLのまま）。

## 実装の核心には触れていないことの確認
ジオフェンスの張り替え方式・バックグラウンド処理の仕組みなど、実装の詳細は本文中に一切記載していない。「自動検知」「個人情報を渡さない」というユーザー便益の説明のみに留めている。
