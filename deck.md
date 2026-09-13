---
marp: true
theme: subako
paginate: true
---

# Subako Hackathon 9/14

Kikuvi Inc.

---

# ようこそ

---

<!-- _class: cards -->

# 開始までのお願い

15:15 頃に開始します。それまでに 3 つお願いします。

- **Wi-Fi** Plug and Play の Wi-Fi に接続（TODO: SSID とパスワード）
- **GitHub アカウント** Codespaces を使うと環境構築なしで始められます
- **Discord** 質問・相談は Subako Discord Server で受け付けます

![w:200](https://api.qrserver.com/v1/create-qr-code/?size=600x600&data=https%3A%2F%2Fdiscord.gg%2Fxxxx)

---

<!-- _class: cover -->

# Subako Hackathon 9/14

Build Agents, Not Infrastructure

###### 2026-09-14 · Kikuvi Inc.

---

# Subakoについて

Concept Movie

---

<!-- _class: cards -->

# Subakoとは

AIエージェントの開発に必要なインフラを提供するプラットフォームです。

- **ハーネス** LLM を呼び、ツールを実行する
- **セッション** 会話の状態を保持するストレージ
- **サンドボックス** LLM が生成したコードを実行する
- **MCP** 認証情報の管理
- **Agent Skills** スキルの管理

インフラと SDK を提供することで、既存のアプリケーションにエージェント機能を組み込み、新たな体験を提供できます。

---

# ハッカソンについて

Subako を使ったエージェント開発を体験するハッカソンです。

- Subako の登録、SDK の導入、スキルの整備、MCP の設定まで、一連の流れを体験できます

---

# 運営メンバー

---

# 本日の流れ

| 時間 | 内容 |
| :---- | :---- |
| 15:00 | イベント準備 |
| 15:15 | Subako の説明とハンズオン |
| 15:45 | 実装時間 |
| 18:00 | 実装終了・作品提出タイム |
| 18:15 | 交流時間 |
| 19:00 | 結果発表 |
| 19:20 | イベント終了 |
| 19:30 | 片付け開始 |

---

<!-- _class: section -->

###### HANDS-ON

# ハンズオン

15:15 – 15:45。普通の React TODO アプリに、皆で一緒にエージェントを組み込みます。

---

<!-- _class: cards -->

###### HANDS-ON

# ハンズオンの流れ

- **1. 環境** リポジトリを Fork し、Codespaces かローカルで開く
- **2. アカウント** CLI を入れて、Subako のアカウント・Org・API キーを作る
- **3. 起動** `.env.local` を設定して TODO アプリを立ち上げる
- **4. 組み込み** SDK を入れて agent を publish し、`useTool` でツールを登録する

---

<!-- _class: cards -->

###### STEP 1

# リポジトリを Fork する

`subako-ai/subako-hackathon-2026-09-14` を自分のアカウントに Fork します。

- **Codespaces** 環境構築に自信がない人はこちら。Fork 先で Code → Codespaces → Create codespace。CLI のインストール・`npm ci`・`.env.local` の準備まで自動で走る
- **ローカル** macOS / Linux に Node.js 22.12 以上を用意して `git clone`。CLI は次のステップで入れる

---

###### STEP 2

# Subako CLI のインストール

Codespaces ではインストール済みです。`subako --version` が通れば次へ。

```sh
curl --proto '=https' --tlsv1.2 -LsSf \
  https://github.com/subako-ai/subako-cli/releases/latest/download/subako-cli-installer.sh | sh
subako --version
```

- コマンドが見つからないときは、新しいターミナルを開く
- Windows でローカル開発したい人は Codespaces を推奨

---

###### STEP 3

# アカウントと Organization の作成

`<ORG HANDLE>` は自分だけの Org 名に置き換えます。

```sh
subako cloud signup --org <ORG HANDLE> --name "Subako Hackathon"
subako login --org <ORG HANDLE>
subako whoami
subako workspace create --name hackathon
subako workspace use hackathon
```

- 表示された URL をブラウザで開いて登録を完了する

---

###### STEP 4

# Subako Credit の取得

本日の参加者には $10 相当の Subako Credit を付与します。下記シートに氏名と Org Handle を記入してください。

```sh
subako org credits      # クレジット残高を表示
subako org show         # 現在の Org 情報を表示
```

![w:200](https://api.qrserver.com/v1/create-qr-code/?size=600x600&data=https%3A%2F%2Fdocs.google.com%2Fspreadsheets%2Fd%2F1trHI1-7zHLJ59BzS0ylmAX3wX1Xk36MWApkT-Kp-Zds%2Fedit%3Fusp%3Dsharing)

---

###### STEP 5

# API キーの発行

アプリから Subako に接続するためのキーです。期限は明日の 0 時にしています。

```sh
subako api-key mint \
  --label "hackathon-2026-09-14" \
  --permission agent.read --permission agent.publish \
  --permission model_provider.read \
  --permission session.create --permission session.read --permission session.manage \
  --expires-at 2026-09-15T00:00:00+09:00
```

- `sbk_ak` で始まるキーが一度だけ表示されます。コピーしてください

---

<!-- _class: code-text -->

###### STEP 6

# TODO アプリの起動

```sh
npm install
npm run setup   # .env.local を作成

# .env.local に記入
SUBAKO_API_KEY=発行したキー
SUBAKO_MODEL_PROVIDER_ID=01a07f90-...
SUBAKO_MODEL_ID=hawk

npm run dev -- todo
```

- ローカルは `http://127.0.0.1:5173`、Codespaces は Ports の 5173
- **`VITE_` が付かないキーはブラウザーへ渡りません。** 使うのは開発サーバーだけ
- キーが空でも TODO の手動操作はできる
- `apps/todo/src/App.tsx` の `add` と `complete` がどこから呼ばれているか探しておく

---

# モデルの使用について

`SUBAKO_MODEL_PROVIDER_ID=01a07f90-238c-7de3-b55e-6fe53de063cf` で Subako 提供のモデルを使えます。推論時に Subako のクレジットを消費します。

```
01a07f90-238c-7de3-b55e-6fe53de063cf  type=platform  format=openai_responses  label=Subako
  model=crow     label=Crow     Balanced capability and cost for everyday tasks.
  model=hawk     label=Hawk     For complex reasoning and coding tasks.
  model=sparrow  label=Sparrow  For simple, high-volume agent tasks.
```

- 一覧は `subako model list`
- OpenAI Responses API 対応のモデルと API キーを登録して使うこともできます。この場合クレジットは消費しません（`subako model-provider create`）

---

<!-- _class: center -->

###### CHECKPOINT

# ここまでのチェック

- TODO の追加・完了ができた
- CLI にログインでき、モデル一覧と残高を確認できた
- API キーを `.env.local` に保存した

---

<!-- _class: cards -->

###### CONCEPT

# Agent と Session

これから出てくる言葉は 3 つだけです。

- **Agent** 何をする人か。モデル・指示（`prompt.md`）・MCP をまとめた設定。`agent:publish` するたびに version が増える
- **Session** 1 つの会話。**作った時点の agent version に結び付きます。** 履歴は Subako 側に残る
- **Client tool** その会話にブラウザーが登録する「アプリの操作」。`useTool` で登録するのがこれ

だから `prompt.md` を直して publish したら、「新しいセッション」で会話を作り直します。押すまでは前の指示のまま続きます。

---

###### STEP 7

# SDK のインストールと publish

```sh
# SDK を todo アプリの依存に追加
npm install --workspace @hackathon/todo \
  @subako-ai/sdk@0.1.1 @subako-ai/react@0.1.1 @subako-ai/assistant-ui@0.1.1 \
  @assistant-ui/react@0.15.19 @assistant-ui/react-markdown@0.14.15 zod@4.6.4

# agent の作成・publish・Origin 許可
npm run agent:publish -- todo
```

- 指示は `agents/todo/prompt.md`。publish が `.env.local` の `SUBAKO_AGENT_TODO` を更新する
- **会話（session）はアプリが作ります。** 出入り口の `src/session.ts` は配置済み
- publish 後は `Ctrl+C` で止めて `npm run dev -- todo` を再起動

---

<!-- _class: compact -->

###### STEP 8

# App.tsx の TODO を上から外す

`apps/todo/src/App.tsx` に ①〜⑥ の TODO コメントを置いてあります。上から順に外していけば動きます。

| | 場所 | やること |
|---|---|---|
| ① | 先頭 | SDK・`session.ts`・`session.css` を import |
| ② | `storageKey` の下 | `SubakoSessionClient` と会話の保存キーを作る |
| ③ | `App` の直前 | `TodoAssistant` と `list_todos` / `add_todo` |
| ④ | ③ の中 | `set_todo_done` を自分で書く → STEP 9 |
| ⑤ | `App` の中 | `useSessionId` で使う会話を用意する |
| ⑥ | `return` の中 | `has-session` とサイドバーを足す |

- **④ 以外はコメントを外すだけ。** 雛形が入っています
- `useTool` が「AI に任せる操作」。`execute` は既存の `add(title)` を呼ぶだけ
- `schema` の Zod が引数の形を AI に伝え、実行前に検証する
- 会話は `useSessionId` が作り、ID を `localStorage` に覚える。API キーは持たない

---

<!-- _class: code-text -->

###### STEP 9

# TODO ④ を自分で書く

```tsx
useTool(client, 'set_todo_done', {
  description:
    '一覧で取得したidのTODOを完了・未完了にする。',
  schema: z.object({
    id: z.string(),
    done: z.boolean(),
  }).strict(),
  execute: ({ id, done }) =>
    JSON.stringify(complete(id, done)),
});
```

- ③ の `useTool` を真似て、既存の `complete(id, done)` を呼ぶツールを足す
- 「サンプルを起動する、テーマを決める、発表を練習する、を追加して」で 3 件増える
- 「サンプルを起動する、は完了した」で完了になる
- 手で別の TODO を完了にしてから「残りを教えて」と聞くと、変更が反映されている

---

<!-- _class: figure -->

###### HANDS-ON

# ① つなぐまで

APIキーはブラウザーへ渡しません。会話の作成と token の発行を開発サーバーに任せます。

![h:415](assets/flow-connect.svg)

---

<!-- _class: figure -->

###### HANDS-ON

# ② 会話のたびに

`useTool` で登録した既存の関数が、LLM から呼ばれて画面を変えます。

![h:415](assets/flow-loop.svg)

---

<!-- _class: section -->

###### HACK TIME

# ハックタイム

15:45 – 18:00。自分の題材でエージェントを作ります。

---

<!-- _class: cards -->

###### HACK TIME

# 2 つのコース

- **EC / マップ** スターターの `ec` か `map` を選び、TODO と同じ手順でエージェントを組み込む。データと prompt を自分の題材に変える。完成例は `ec-coffee` / `map-coffee`
- **上級: 自分のアプリ** 既存のアプリに Subako Agent を組み込む。流れは同じ。API キー → publish → SDK → `useSessionId` → `useTool` → チャット

---

<!-- _class: compact -->

###### HACK TIME

# EC / マップの進め方

| | EC | マップ |
|---|---|---|
| スターター / 完成例 | `apps/ec` / `apps/ec-coffee` | `apps/map` / `apps/map-coffee` |
| データ | `apps/ec/data/catalog.json` | `apps/map/src/data.json` |
| prompt | `agents/ec/prompt.md` | `agents/map/prompt.md` |
| 最初に読むツール | `search_items` | `get_places` |
| 最初に画面を変えるツール | `show_items` | `show_candidates` |

- SDK インストール → `npm run agent:publish -- ec` → `npm run dev -- ec`。TODO と同じ 3 コマンド
- 16:30 までに「候補を表示して選ぶ」まで動かすのが目安

---

<!-- _class: cards -->

###### HACK TIME

# 工夫のヒント

- **データ** `catalog.json` / `data.json` を自分の業界に置き換える。`metadata` に画面に出さない判断材料を入れる
- **prompt** `agents/<app>/prompt.md` を書き換えて publish。**そのあと「新しいセッション」を押すと新しい指示で会話が始まります**
- **MCP** `presets/mcp/exa.json`（Web 検索）や `eris.json`（現在の天気）を `agents/<app>/mcp.json` にコピー
- **ツール・UI** カートや訪問順の操作、比較表、独自 API の追加など

---

###### SUBMISSION

# 提出

18:00 までに、代表者が Google フォームから提出します。

- チーム名・メンバー・作品名
- 誰の何を助けるか（100 字程度）
- 60 秒以内のデモ動画 1 本。画面録画のままで OK
- 工夫した点（200 字程度）
- TODO: 提出フォームの URL / QR コードを記載

---

<!-- _class: cards -->

###### PRIZES

# 賞

審査は、利用者への価値・動く体験・工夫を見ます。副賞は Subako の有償クレジットです（TODO: 金額を記載）。

- **最優秀賞** 価値・体験・工夫の総合
- **ベストエクスペリエンス賞** いちばん気持ちよく動いた作品
- **ベストアイデア賞** 題材の選び方と工夫が光る作品
