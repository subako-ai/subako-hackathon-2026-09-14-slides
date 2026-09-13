# Subako Hackathon Slides

Marp で作る運営スライド。デザインは `Kikuvi-Inc/subako-pitch` のデザインシステムを移植した
`themes/subako.css` で、投資家向けなど作り込むデッキは Keynote テーマ（`subako-pitch/keynote/`）を使う。

```bash
npm install      # 初回だけ
npm run dev      # http://localhost:8080 でプレビュー（保存で自動リロード）
npm run pdf      # deck.md -> deck.pdf
npm run sample   # sample.md -> sample.pdf（テーマの見本）
```

VS Code の Marp 拡張を入れていれば `.vscode/settings.json` でテーマが読まれるので、
`npm run dev` なしでプレビューできる。

## 書きかた

front matter に 3 行。あとは普通の Markdown。

```markdown
---
marp: true
theme: subako
paginate: true
---

###### ANNOUNCEMENT     <- h6 がアイブロウ（六角形マーカー付き・mono）
# スライドの見出し        <- h1 がスライドタイトル
見出し直後の段落はリード文になる。

ここからは本文。
```

`---` でスライドを区切る。`#` 直後の段落だけがリード文で、それ以降は普通の本文。

## レイアウト

スライド先頭に `<!-- _class: ... -->` を置く。クラスは空白区切りで併用できる（`cards dark` など）。

| クラス | 用途 |
|---|---|
| （無指定） | 既定。アイブロウ + 見出し + リード + 本文 |
| `cover` | 表紙。左に縦組ロゴ、右にタイトル。`h6` は下部のメタ情報になる |
| `section` | ダーク地の扉。章の切り替え |
| `statement` | 64px の大見出し一枚 |
| `cards` | `ul` を paper のカードに。**列数は枚数から自動**（2〜4 はそのまま、5 で一段小さく、6 で 3 列 2 段） |
| `cards-2` … `cards-5` | 列数を明示したいとき |
| `code-text` | コード左・箇条書き右の 2 段組 |
| `media` | 枠付きの画像一枚 |
| `figure` | 図版一枚。`![h:400](...)` で高さを決めると、縦横比のまま中央に載る |
| `thanks` | 中央寄せの締め |
| `dark` | 上記への修飾子。地が ink になる |
| `center` | 内容を上下中央に寄せる |
| `compact` | 箇条書きと表を一段小さく（項目が多いとき） |
| `paper` | 地を canvas から paper に上げる |
| `plain` | 著作権表記とページ番号を消す |

カードはこう書く。`**強調**` で始めるとそこがカード見出しになる。

```markdown
<!-- _class: cards -->

# 5 つのコンポーネント

- **harness** エージェントを動かす器
- **session** 会話と作業の状態を保持する単位
- **sandbox** コードを実際に走らせる隔離環境
```

全レイアウトの見本は `sample.md`。`npm run sample` で PDF になる。

## 図をつくるとき

Marp は Mermaid を描画しない（`language-mermaid` のコードブロックのまま出る）ので、
ソースを `assets/*.mmd` に置き、SVG に焼いてから `![h:415](assets/xxx.svg)` で貼る。
高さを指定して縦横比のまま中央に置くため、スライドには `figure` クラスを使う。

`.mmd` を直したら、この手順で焼き直す。色と級数は `tools/render-mermaid.html` が持っている。

```bash
cp assets/*.mmd tools/render-mermaid.html /tmp/
(cd /tmp && python3 -m http.server 8899 --bind 127.0.0.1 &)

agent-browser open "http://127.0.0.1:8899/render-mermaid.html?f=flow-loop"
agent-browser wait 4000
agent-browser eval "document.querySelector('#out svg').outerHTML" > /tmp/out.raw
```

`/tmp/out.raw` は JSON 文字列なので、`<svg` 以降を取り出し、`width="100%"` と
`max-width` を外して `assets/<名前>.svg` に保存する。この2つを残すと、
高さ指定が効かず図が歪む。

## テーマを直すとき

`themes/subako.css` の `:root` にトークンが並んでいる。色・級数・余白は
`subako-pitch/ref/Subako-Assets/Subako_DesignSystem/Subako-DesignSystem.html` が原典なので、
digit を変えるときはそちらと突き合わせる。

ハマりどころ:

- **`content: none` は効かない。** Marpit の CSS 最適化が宣言ごと落とす。`content: ""` を使う
- **カードの `li` を `display: flex` にしない。** インラインの `<code>` がフレックスアイテム化して全幅に伸びる
- **書体は Google Fonts から `@import` している。** オフラインだとヒラギノにフォールバックする
- **ブランドの SVG は data URI で埋め込んである。** ファイル参照だと PDF 書き出しに `--allow-local-files` が要るため
- **`---` の前には空行を入れる。** 直前が本文だと Markdown の setext 見出しとして食われ、区切りが消えてスライドが 1 枚減る

左下の著作権表記はデッキ側で変えられる。

```markdown
<!-- _style: "section { --copy: 'Subako Hackathon 2026'; }" -->
```
