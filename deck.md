---
marp: true
theme: subako
paginate: true
---

<!-- _class: cards -->

# 開始までのお願い

15:15 頃に開始します。それまでに 3 つお願いします。

- **Wi-Fi** Plug and Play の Wi-Fi に接続<br>SSID: `pnpj_guest_5G`<br>パスワード: `20200406`
- **GitHub アカウント** Codespaces を使うと環境構築なしで始められます
- **Discord** 質問・相談は [Subako Discord Server](https://discord.gg/PkDFYjyyHX) で受け付けます。<br><br><center>![w:200](https://api.qrserver.com/v1/create-qr-code/?size=600x600&data=https%3A%2F%2Fdiscord.gg%2FPkDFYjyyHX)</center>

---

<!-- _class: cover -->

# Subako Hackathon 9/14

Build Agents, Not Infrastructure

###### 2026-09-14 · Kikuvi Inc.

---

# Subakoについて

[Concept Movie](https://www.youtube.com/watch?v=Vpt5voDm1j8)

---

<!-- _class: hex -->

###### Platform

# Subakoとは

AIエージェントの開発に必要なインフラを提供するプラットフォームです。

- ![](assets/icons/harness.svg) **ハーネス** *harness* LLM を呼び、ツールを実行する
- ![](assets/icons/sessions.svg) **セッション** *sessions* 会話の状態を保持するストレージ
- ![](assets/icons/sandbox.svg) **サンドボックス** *sandbox* LLM が生成したコードを実行する
- ![](assets/icons/vault.svg) **Vault** *vault* 認証情報の管理
- ![](assets/icons/skills.svg) **Agent Skills** *skills* スキルの管理

インフラと SDK を提供することで、既存のアプリケーションにエージェント機能を組み込み、新たな体験を提供できます。

---

# ハッカソンについて

Subako を使ったエージェント開発を体験するハッカソンです。

- Subako の登録、SDK の導入、スキルの整備、MCP の設定まで、一連の流れを体験できます

---

<!-- _class: team -->

# 運営メンバー

本日の運営は 8 名です。詰まったら近くのメンバーか Discord にどうぞ。

- ![](assets/members/kento-sato.png) **Kento Sato** 佐藤 拳斗 *Founder / CEO*
- ![](assets/members/shun-kashiwa.png) **Shun Kashiwa** 柏 舜 *Founding Engineer*
- ![](assets/members/masa-ishihara.png) **Masa Ishihara** 石原 正宗 *Founding AI/ML Engineer*
- ![](assets/members/takumi-okoshi.png) **Takumi Okoshi** 大越 拓実 *AI/ML Engineer*
- ![](assets/members/taka-nagai.png) **Takayuki Nagai** 長井 崇行 *Founding Designer*
- ![](assets/members/hironori-kawamoto.png) **Hironori Kawamoto** 川本 博詔 *Software Engineer*
- ![](assets/members/kosei-matsuyama.png) **Kosei Matsuyama** *Software Engineer*
- ![](assets/members/ryuji-miyasaka.png) **Ryuji (RJ) Miyasaka**

---

# 代表挨拶

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

普通の React TODO アプリに、皆で一緒にエージェントを組み込みます。

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

[`subako-ai/subako-hackathon-2026-09-14`](https://github.com/subako-ai/subako-hackathon-2026-09-14) を自分のアカウントに Fork します。

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

本日の参加者には $20 相当の Subako Credit を付与します。下記シートに氏名と Org Handle を記入してください。

```sh
subako org credits      # クレジット残高を表示
subako org show         # 現在の Org 情報を表示
```

![w:200](https://api.qrserver.com/v1/create-qr-code/?size=600x600&data=https%3A%2F%2Fdocs.google.com%2Fspreadsheets%2Fd%2F1trHI1-7zHLJ59BzS0ylmAX3wX1Xk36MWApkT-Kp-Zds%2Fedit%3Fusp%3Dsharing)

https://docs.google.com/spreadsheets/d/1trHI1-7zHLJ59BzS0ylmAX3wX1Xk36MWApkT-Kp-Zds/edit?usp=sharing

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
SUBAKO_MODEL_PROVIDER_ID=01a07f90-238c-7de3-b55e-6fe53de063cf
SUBAKO_MODEL_ID=hawk

npm run dev -- todo
```

- ローカルは `http://127.0.0.1:5173`、Codespaces は Ports の 5173
- **`VITE_` が付かないキーはブラウザーへ渡りません。** 使うのは開発サーバーだけ
- キーが空でも TODO の手動操作はできる。サイドバーは空のまま
- 以降のコマンドは **2 つ目のターミナル** で打つ。`Ctrl+C` で止めなくてよい
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

# Agent, Session, Tool

Subakoを使う上で重要になる3つの概念を整理します。

- **Agent** 何をする人か。モデル・指示（`prompt.md`）・MCP をまとめた設定。`agent:publish` するたびに version が増える。
- **Session** 1つの会話履歴。Agentに紐付く。履歴はSubako側に残る。
- **Tool** Agentが行うことのできる操作。*Client* toolsは、そのうちブラウザなどクライアントの関数をエージェントに提供する仕組みを指します。

---

###### STEP 7

# SDK のインストールと publish

```sh
# SDK を todo アプリの依存に追加
npm install --workspace @hackathon/todo \
  @subako-ai/sdk@0.1.2 @subako-ai/react@0.1.2 @subako-ai/assistant-ui@0.1.2 \
  @assistant-ui/react@0.15.19 @assistant-ui/react-markdown@0.14.15 zod@4.6.4

# agent の作成・publish
npm run agent:publish -- todo
```

- `agents/todo/prompt.md` がAgentのシステムプロンプトになります。
- `subako agent list` を実行すると作成されたAgentを確認できます。
- `.env.local` が変わったので、開発サーバーを `Ctrl+C` で止めて `npm run dev -- todo` を再起動してください。

---

<!-- _class: compact -->

###### STEP 8

# App.tsx の TODO を上から外す

`apps/todo/src/App.tsx` に ①〜⑥ の TODO コメントを置いてあります。上から順に外していけば動きます。サイドバーの枠は最初からあります。

| | 場所 | やること |
|---|---|---|
| ① | 先頭 | SDK と `session.ts` を import（`session.css` は済み） |
| ② | `storageKey` の下 | `SubakoSessionClient` と会話の保存キーを作る |
| ③ | `App` の直前 | `TodoAssistant` と `list_todos` / `add_todo` |
| ④ | ③ の中 | `set_todo_done` を自分で書く → STEP 9 |
| ⑤ | `App` の中 | `useSessionId` で使う会話を用意する |
| ⑥ | サイドバーの中 | `<p>` と、`{/*`・`*/}` の行を消す |

- **④ 以外はコメントを外すだけです。**
- `useTool` が「AI に任せる操作」。`execute` は既存の `add(title)` を呼ぶだけ
- `schema` の Zod が引数の形を AI に伝え、実行前に検証する
- ブラウザーは API キーを持たない。会話の ID は `localStorage`、接続トークンは開発サーバーから受け取る

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

18:00まで。自分の題材でエージェントを作ります。

---

<!-- _class: cards -->

###### HACK TIME

# 2 つのコース

- **サンプルアプリを使う** スターターの `ec` か `map` を選び、TODO と同じ手順でエージェントを組み込む。データと prompt を自分の題材に変える。`ec-coffee` / `map-coffee` を参考として、データを置き換えたり、プロンプトを変えたり、ツールを追加したりする。
- **上級: 自分のアプリ** 既存のアプリに Subako Agent を組み込む。流れは同じ。API キー → publish → SDK → `useSessionId` → `useTool` → チャット

---

<!-- _class: shots -->

###### SAMPLE APPS

# 2 つのスターター

TODO と同じ形のアプリを 2 つ用意しました。画面もデータもできていて、**サイドバーの枠だけが空いています**。

- ![](assets/screens/ec-overview.png) **EC** `apps/ec` サンプル商品 4 点。比較・カート・購入確認まで手で動きます
- ![](assets/screens/map-overview.png) **マップ** `apps/map` 渋谷周辺のサンプル地点 4 つ。候補と訪問順を地図に出せます

---

<!-- _class: shot -->

###### 作例 · EC ①

# コーヒーの EC にしてみる

`apps/ec-coffee` は、スターターのデータを 10 種のコーヒー豆に差し替えたものです。

![](assets/screens/ec-coffee-overview.png)

---

<!-- _class: shot -->

###### 作例 · EC ②

# 好みと予算を伝える

「酸味は控えめ、ミルクに合う豆。予算 2,500 円で 3 種の飲み比べセットを」。一覧が 3 点に絞られ、カートに入り、**理由が一言ずつ**返ります。

![](assets/screens/ec-coffee-ai-result.png)

---

<!-- _class: shot -->

###### 作例 · MAP ①

# 渋谷のコーヒー屋を巡る

`apps/map-coffee` は、地点を渋谷の 6 店舗に差し替えたものです。実在の店舗情報と、事前に取得した徒歩経路を持っています。地図は OpenStreetMap。

![](assets/screens/map-coffee-overview.png)

---

<!-- _class: shot -->

###### 作例 · MAP ②

# 条件を言うとコースになる

「緑のあるカフェとコーヒースタンドを巡る 3 店のコースを作って」。候補・訪問順・徒歩経路が地図に描かれ、**合計約 32 分**と返ります。

![](assets/screens/map-coffee-ai-result.png)

---

<!-- _class: shot -->

###### 作例 · MAP ③

# 提案の上から、人が調整する

気になった店を開いて詳細を見る。固定する、順番を入れ替える、外す。AI が出すのは、**人が書き換えられる下書き**です。

![](assets/screens/map-coffee-place-detail.png)

---

<!-- _class: cards -->

###### 作例

# 作例で足したもの

ここまでの 6 枚で、スターターから変えたのはこの 3 つだけです。

- **データ** コーヒー豆 10 種、渋谷の 6 店舗。`metadata` に酸味・焙煎度・店の特徴など、**画面には出さない判断材料**を入れてあります
- **prompt** 「酸味・焙煎度・産地・予算で選ぶ」「特徴・徒歩時間・滞在時間で並べる」。題材の言葉で数行を書き直しただけです
- **ツール** 読むツールと、画面を変えるツール。`ec-coffee` は 9 個、`map-coffee` は 5 個。どれも既存の関数を `useTool` で包んだだけです

---

<!-- _class: compact -->

###### HACK TIME

# EC / マップの進め方

| | EC | マップ |
|---|---|---|
| スターター / 完成例 | `apps/ec` / `apps/ec-coffee` | `apps/map` / `apps/map-coffee` |
| 3 コマンドの `<app>` | `ec`（`--workspace @hackathon/ec`） | `map`（`--workspace @hackathon/map`） |
| ポート | 5177 | 5175 |
| データ | `apps/ec/data/catalog.json` | `apps/map/src/data.json` |
| prompt | `agents/ec/prompt.md` | `agents/map/prompt.md` |
| 画面の操作関数 | `useCatalog` が返す `catalog` | `useMapApp` が返す `app` |
| 最初に読むツール | `search_items` | `get_places` |
| 最初に画面を変えるツール | `show_items` | `show_candidates` |

- EC / マップの `App.tsx` にも TODO ①〜⑥ があります。
- `execute` は画面のボタンと同じ関数を呼ぶ。現在の状態は `catalog.getState()` / `app.getState()` で読む
- 完成例の全ツールと diff は、リポジトリの `README.md` と `docs/answers.md`

---

<!-- _class: compact -->

###### HACK TIME

# 2 時間の目安

| 時刻 | やること | 見るもの |
|---|---|---|
| 15:45 | 3 コマンドで起動し、手動操作を確認 | |
| 15:55 | ①②⑤⑥ を書いて、空の会話をサイドバーに出す | 自分の `apps/todo/src/App.tsx` |
| 16:15 | ③ 読むツール 1 つ。AI に「何がある？」と聞ける | `catalog.getState()` / `app.getState()` |
| 16:30 | ③ 画面を変えるツール 1 つ。**候補を表示して選ぶ、まで動く** | `catalog.showItems` / `app.showCandidates` |
| 16:45 | データを自分の題材に置き換え。項目名は変えず `metadata` に足す | `catalog.json` / `data.json` |
| 17:10 | prompt を書き換え → publish → 「新しいセッション」 | `agents/<app>/prompt.md` |
| 17:20 | ④ 操作ツール（カート・訪問順・固定）、MCP、画面文言。**18:00 で実装終了** | 完成例と `docs/answers.md` |
| 18:00 | 60 秒のデモ動画を撮って、18:15 までにフォームから提出 | 提出スライド |

- マップの出発地は渋谷駅で固定。別の街にするなら `domain.ts` の `SHIBUYA_STATION` も変える

---

<!-- _class: cards -->

###### HACK TIME

# 工夫のヒント

- **データ** `catalog.json` / `data.json` を自分の業界に置き換える。`metadata` に画面に出さない判断材料を入れる
- **prompt** `agents/<app>/prompt.md` を書き換えて publish。**そのあと「新しいセッション」を押すと新しい指示で会話が始まります**
- **MCP** `presets/mcp/exa.json`（Web 検索）や `eris.json`（現在の天気）を `agents/<app>/mcp.json` にコピーして publish し直し、「新しいセッション」
- **ツール・UI** カートや訪問順の操作、比較表、独自 API の追加など

---

###### SUBMISSION

# 提出

18:15 までに、代表者が Google フォームから提出します。

- フォームに提出する内容
    - チーム名・メンバー・作品名
    - 誰の何を助けるか（100 字程度）
    - 60 秒以内のデモ動画 1 本。画面録画のままで OK
    - 工夫した点・困った点（200 字程度）
- 作品は後ほど全体で共有します
- https://forms.gle/cEZMCG81VA7DpU2p8
    - ![w:140](https://api.qrserver.com/v1/create-qr-code/?size=600x600&data=https%3A%2F%2Fforms.gle%2FcEZMCG81VA7DpU2p8)

---

<!-- _class: cards -->

###### PRIZES

# 賞

審査は、利用者への価値・動く体験・工夫を見ます。副賞は Subako の有償クレジットです。

- **最優秀賞(30,000 SC)** 価値・体験・工夫の総合
- **ベストエクスペリエンス賞(10,000 SC)** いちばん気持ちよく動いた作品
- **ベストアイデア賞(10,000 SC)** 題材の選び方と工夫が光る作品
