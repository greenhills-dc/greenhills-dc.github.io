# greenhills-dc アプリ紹介サイト

公開 URL: https://greenhills-dc.github.io/ （GitHub Pages・main ブランチのルートを配信）

静的 HTML＋CSS のみです（JavaScript・外部 CDN・Web フォント・解析タグなし）。CSS は `style.css` 1 本です。

## デザイン（v2・2026-10-28）

- デザインの原本は `../design_src_20261028/`（`index.html`・`style.css`。院長が用意したもの。手を入れない控え）。`index.html` は原本に head の技術タグ（CSP・referrer・canonical・OGP・JSON-LD・favicon）、ヒーロー見出しの改行対策（`<span class="line">`）、画像の `width`/`height` だけを足したものです。
- `style.css` の前半は原本のまま。原本から直した行には `/* v2 fix */` を付けています（ヒーロー見出しの文字サイズと改行、重ねカードの文字位置・幅・文字サイズ）。
- アプリページ（`apps/*.html`）と `404.html` の CSS は、`style.css` 末尾の `/* ---- app pages (v2) ---- */` 以降にまとめています。色・書体・罫線・ピル・下線リンクは原本のトークンとクラス（`.eyebrow`・`.platforms`・`.hero-link`・`.detail-link`・`.about-copy`）を使い回しています。
- `img/favicon.svg` は原本のブランドマーク（`.brand-mark`）に合わせた、深緑 `#18332f` の円にクリーム `#fffefb` の Georgia 斜体「g」です。
- 見出しや文言を直すときは、まず原本の調子（です・ます、明朝の見出し、アプリ名は Georgia）に合わせてください。

## ファイル構成

- `index.html` … アプリ一覧（ハブ）
- `apps/<slug>.html` … アプリごとのページ（custodia／cria／d-plot／stockage／laborder／dental-cloud-share）
- `style.css` … 共通スタイル（前半＝原本、末尾＝アプリページ用の v2 ブロック）
- `img/<slug>.webp`・`img/<slug>.jpg` … PR 画像（1600 幅）。`img/favicon.svg` … ファビコン
- `sitemap.xml`・`robots.txt`・`404.html`・`.nojekyll`

## 画像の差し替え

元画像（PNG）から JPEG と WebP を作り、`img/` の同名ファイルを置き換えます。

```sh
# JPEG（幅 1600・品質 82）
sips -s format jpeg -s formatOptions 82 --resampleWidth 1600 元画像.png --out img/<slug>.jpg
# WebP（幅 1600・品質 80）
cwebp -q 80 -resize 1600 900 元画像.png -o img/<slug>.webp   # 高さは JPEG の実寸に合わせる（0 にすると 1px ずれることがある）
# 実寸の確認（HTML の width/height と合わせる）
sips -g pixelWidth -g pixelHeight img/<slug>.jpg
```

高さが変わった場合は、`index.html` と `apps/<slug>.html` の `<img>` の `width`/`height`、`og:image:width`/`og:image:height` を実寸に合わせて直します。

## アプリの追加

1. `apps/` の既存ページを 1 つ複製して `apps/<新slug>.html` にし、title（「アプリ名｜一言｜greenhills-dc」）・description・canonical・OGP・JSON-LD・本文・`.eyebrow` の番号を書き換える（description は 80〜120 字・ページごとに固有）。
2. `index.html` のアプリ一覧にカード（`article.app-card`）を 1 枚足す。画像の `<img>` には `width`/`height` を実寸で付ける。
3. 各アプリページの「他のアプリ」帯（`.other-list`）に行を足す（番号・名前・キッカー・くわしく見る）。
4. `sitemap.xml` に `<url>` を 1 行足す。

## sitemap の lastmod

ページの内容を変えたら、`sitemap.xml` の該当 URL の `<lastmod>` を更新日（YYYY-MM-DD）に直します。

## Search Console

各ページの head にある `<!-- Search Console の HTML タグをここに貼る -->` の位置に、取得した `google-site-verification` の meta タグを貼ります（index.html だけで足ります）。

## 公開

main に push すると GitHub Pages が自動で反映します（数分かかります）。
