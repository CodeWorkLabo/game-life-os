# Freakdex — Phase 1.5 開発計画 & Claude Code 依頼文集

このドキュメントには2つのものが入っています:

1. **Part 1**: 新しい `CLAUDE.md` の内容(まるごと差し替え用)
2. **Part 2**: Claude Code に渡す段階依頼文5つ(コピペで使う)

---

# Part 1: CLAUDE.md(差し替え用・全文)

> 以下の内容で `C:\Projects\CLAUDE.md` を上書きしてください。
> 一度ブラウザでこのドキュメントの内容をコピーして、VSCodeでCLAUDE.mdを開いて全選択(Ctrl+A)→ペースト(Ctrl+V)→保存(Ctrl+S)で完了。

```markdown
# Freakdex — Claude Code Instructions

## プロジェクト概要

**Freakdex(フリークデックス)**は、ゲーマー向けの「自己紹介カード生成 + プロフィール公開」サービス。

サービスの一言: **「Your gamer self, indexed.」 / 「ゲーマーとしての、あなたの図鑑。」**

## プロダクトの主軸

**「ゲーマー名刺メーカー + ゲーム人生のミニ振り返り」**

- **メイン**: SNSで使える自己紹介カード画像(1200×630 PNG)を3分で生成
- **サブ**: その画像のURLをタップすると、詳細なプロフィールページが見える
- **武器**: AIによる「ゲーマーアーキタイプ」診断(MBTIのようなバズ要素)

### 利用シーン

SNS(主にX/Twitter)で「#フレンド募集」のようなタグと共に、自分のゲーマー像が伝わるカード画像を投稿する。
興味を持った人がURLをタップすると、より詳細なプロフィールを見られる、という導線。

### コアコンセプト

- **データではなく記憶**: Steamにあるプレイ時間・実績ではなく、「なぜ好きだったか」「どんな思い出があるか」を残す
- **3分で名刺、深く知りたい人にはプロフィール**: 画像で広く拡散、URLで深い理解

## 現在のフェーズ

**Phase 1.5: ゲーマー名刺メーカーへのピボット**

技術スタックは Phase 1 を継続:
- HTML + CSS + JavaScript(1ファイル完結)
- LocalStorage のみ
- React / Next.js / DB はまだ導入しない

## ファイル構成

```
C:\Projects\
├── index.html       # メインのプロトタイプ
├── package.json
├── README.md
├── CLAUDE.md        # このファイル
└── .gitignore
```

## ネーミング規約(Phase 1.5 で確定)

| 旧 | 新 |
|----|----|
| GameLifeOS | Freakdex |
| あなたのゲーム人生 | あなたのゲーマー像 |
| Game Life Profile | Player Card |
| AI Profile | Archetype |
| Library | Played |
| 2026 Wrapped | 2026 Recap |
| New Record | + Log |
| Top Emotion(統計4枚目) | Archetype |
| gamelife.app | freakdex.app |

英語キーは可能な限り変えず、表示テキストだけを差し替える方針(LocalStorageキー `gamelife.records.v1` などはそのまま)。

## LocalStorageキー(変更しない)

- `gamelife.records.v1` — ゲーム記録の配列
- `gamelife.profile.v1` — `{ avatarDataUrl }` プロフィール画像
- `gamelife.records.v1.seeded` — デモデータ投入済みフラグ

**重要**: キー名を変えるとユーザーの既存データが消えるので、変えない。

## index.html 内部の論理構造

- **Storage Layer**: `loadRecords` / `saveRecords` / `loadProfile` / `saveProfile`
- **State**: `state = { records, profile, rating, emotions }`
- **Render**: `renderStats` / `renderLibrary` / `renderAIProfile` / `renderWrapped` / `renderAvatar`
- **Form handlers**: 評価 / 感情タグ / アバター画像
- **Demo seed**: 初回起動時にサンプル4本(Hollow Knight / Outer Wilds / Stardew Valley / ELDEN RING)

## Phase 1.5 のスコープ(5タスク)

1. **重要バグ修正** — `render()` に `renderAvatar()` を追加、プレイ時間に上限チェック
2. **ネーミング刷新** — 表示テキストを Freakdex 体系に統一
3. **TOP EMOTION → Archetype 昇格** — 統計4枚目を Archetype に変更
4. **画像クロップ機能** — アバターアップロード時にクロップモーダル
5. **シェア用画像生成** — Player Card(1200×630 PNG)を生成・ダウンロード

### Phase 2 送り(やらない)

- 背景テーマ複数化(5種類)
- 画像クロップの回転機能
- Replay 長尺(縦スクロール紙芝居)
- ライブラリのスクリーンショット添付
- 細かいUX改善

## 開発ルール

### やってほしいこと

- 変更前に **必ず影響範囲を説明** してから手を動かす
- 大きい変更は **複数案を提示** してから選ばせる
- CSS変数(`:root` の `--bg-base` など)を活用して一貫性を保つ
- 初心者向けに **コメントを丁寧に** 残す
- ファイル分割は事前に相談する

### やらないでほしいこと

- Phase 1.5 で React / TypeScript / npm パッケージ追加を提案しない
- 一度に大量の変更をしない(小さくコミットできる単位で)
- 既存のデザイントークン(色・フォント)を勝手に変えない
- LocalStorageキーの名前を変えない
- スコープ外のことを勝手に追加しない(背景テーマ複数化、Replay長尺、スクリーンショットなど)

### コーディング規約

- インデント: 2スペース
- セミコロン: あり
- 文字列: シングルクォート優先
- 関数定義: `function` 宣言 or アロー関数、どちらでも可
- 日本語コメントOK

### コミットメッセージ

Conventional Commits 形式:
- `feat:` 新機能
- `fix:` バグ修正
- `style:` 見た目だけの変更
- `refactor:` 動作は変えないコード整理
- `chore:` その他

例: `feat: add player card image generator`

## 開発フロー

1. ユーザーがタスクを1つずつ依頼
2. Claude Code が影響範囲を説明
3. ユーザーOKで実装
4. ユーザーがLive Serverで動作確認
5. OKならコミット + push
6. 次のタスクへ

## アーキタイプ仕様(変更なし)

5パターンの判定ロジックは Phase 1 から継続:
- The Deep Diver(深淵潜行型) — 平均プレイ時間 > 50h
- The Melancholic(余韻探求型) — 最頻感情が lonely または bittersweet
- The Adrenaline Seeker(鼓動追求型) — 最頻感情が exciting または tense
- The Curator(厳選収集型) — 平均評価 ≥ 4.5
- The Wanderer(漂流型) — 上記以外

## 重要な思想

機能追加を迷ったとき、毎回この問いに戻る:

> **「これはフレンド募集カードに必要? それとも振り返り機能?」**

前者なら Phase 1.5 で、後者なら Phase 2 で。
```

---

# Part 2: Claude Code 段階依頼文(5タスク)

> 以下の依頼文を、**1つずつ順番に** Claude Code にコピペして実行してください。
> 各タスクが完了 → 動作確認 → コミット & push → 次のタスク、という流れで進めます。

## 🎬 開幕: まず Claude Code にプロジェクト状況を理解させる

```
CLAUDE.md を読み直してください。
特に「現在のフェーズ」が Phase 1.5 になり、プロダクトのピボットがあったこと、
ネーミング規約が新しくなっていることを理解してください。

その上で、現在の index.html を読んで、Phase 1.5 で必要な変更点を箇条書きで
まとめてください。まだコードは変更しないでください。
```

Claude Code が現状把握できたら、次のタスクへ進む。

---

## ▶ タスク1: 重要バグ修正

```
タスク1: 重要バグの修正を2件お願いします。

【バグB1】
render() 関数に renderAvatar() の呼び出しが欠けている。
現状では renderStats / renderLibrary / renderAIProfile / renderWrapped が呼ばれるだけで、
renderAvatar が含まれていない。これを追加してください。

【バグU2】
プレイ時間の入力欄(input#hours)に上限チェックが無く、99999のような非現実的な値が
入力できてしまう。max属性で 10000 を上限に設定し、フォーム送信時にもJavaScript側で
チェックして、超えていたらalertで知らせて中断するようにしてください。

実装前に修正箇所(行番号)と修正内容を簡潔に説明してから着手してください。
```

### 完了後にやること
- ブラウザで動作確認(プレイ時間に大きい数を入れるとはじかれるか)
- `git add` → `commit` → `push`
- コミットメッセージ例: `fix: add renderAvatar to render() and add play hours upper limit`

---

## ▶ タスク2: ネーミング刷新

```
タスク2: ネーミングを刷新します。

CLAUDE.mdの「ネーミング規約」セクションの表に従って、index.html 内の表示テキストを
すべて新しい名称に置き換えてください。

【特に置き換える箇所】
- ページタイトル(<title>)
- ロゴテキスト「Game Life OS」→「Freakdex」
- ナビゲーションのリンクラベル
- ヒーロー見出し「あなたのゲーム人生。」→「あなたのゲーマー像。」
- ヒーロータグ「Game Life Profile / gamelife.app/you」→「Player Card / freakdex.app/you」
- セクション見出し「AI Profile」→「Archetype」
- セクション見出し「Library」→「Played」
- セクション見出し「2026 Wrapped」→「2026 Recap」
- フォーム見出し「New Record」→「+ Log」
- フォーム見出し「Memory / What did you feel?」など、英語表記はそのままでよい
- フッターの「Game Life OS · prototype v0.1」→「Freakdex · prototype v0.2」

【重要な制約】
- LocalStorageキー(gamelife.records.v1 など)は変更しないこと
- JavaScript の変数名、関数名、ID、クラス名は変更しないこと
- 表示テキストだけを変える(コード構造を壊さない)
- 日本語の「あなたのゲーマー像。」の "ゲーマー像" 部分を、既存のem要素を使って
  アクセントカラー(--magenta)で強調する形にしてください

変更前に、置き換える文字列の一覧を提示してください。
```

### 完了後にやること
- ブラウザで見た目の確認(用語が新しくなっているか)
- LocalStorageが壊れていないかも確認(リロードしてゲーム記録が残っていればOK)
- コミット例: `style: rename UI to Freakdex naming convention`

---

## ▶ タスク3: TOP EMOTION → Archetype 昇格

```
タスク3: ヘッダー統計の4枚目「Top Emotion」を「Archetype」に置き換えてください。

【現状】
統計カードが4枚並んでいて、4枚目に最頻感情(Top Emotion)が表示されている。
HTML上のID: stat-emotion / ラベル: "Top Emotion"

【変更内容】
4枚目を「Archetype」に変更:
- ラベル: "Archetype"
- 値: AIが判定したアーキタイプ名(英語の方、例: "The Deep Diver")
- 統計データが3本未満のときは "—"(現状の renderAIProfile と同じ閾値)
- フォントサイズは小さめにして他のカードと揃える(現状のTop Emotionと同程度)

【実装方針】
- 既存の renderStats 関数の中で、アーキタイプ判定ロジックを呼ぶか、
  renderAIProfile 内で計算した archetype を共有変数に持つかのどちらかを提案してください。
  共通化したほうがメンテしやすいので、軽くリファクタしてもOKです。
- 感情の集計コード自体は残してOK(他で使うかもしれないので)。
  Top Emotion の表示部分だけを置き換える。
- アクセントカラーは現状の4枚目と同じ(緑のままでOK、または紫 --purple に変えてもOK)

変更前に、アーキタイプ判定ロジックをどう共通化するか提案してください。
```

### 完了後にやること
- ブラウザで4枚目が Archetype 表示になっているか
- デモデータの状態で「The Deep Diver」と出ているはず
- コミット例: `feat: replace top emotion stat with archetype`

---

## ▶ タスク4: 画像クロップ機能

```
タスク4: プロフィール画像アップロード時に、クロップモーダルを開く機能を追加します。

【ユーザー体験】
1. アバターをクリックしてファイル選択
2. 画像を選ぶと、画面中央にクロップモーダルが自動で開く
3. モーダル内に大きい画像プレビュー + 円形のクロップガイド(オーバーレイ)
4. ユーザーは画像をドラッグして位置を調整、ホイールまたはスライダーでズーム
5. 「確定」ボタンで、円形にクロップされた256×256のJPEGを保存(現状と同じ仕様)
6. 「キャンセル」ボタンで何も変更せずに閉じる

【操作仕様】
- ズーム: PCはマウスホイール + スライダーUI、スマホはピンチイン/アウト + スライダーUI(両方対応)
- ドラッグ: PCはマウスドラッグ、スマホはタッチドラッグ(Pointer Eventsで一本化)
- クロップ形状: 円形固定(四角切り替えなし)
- ズーム範囲: 1.0倍〜3.0倍(初期値1.0、画像が正方形枠より小さい場合は枠いっぱいに合わせる)
- 画像が枠外に出ないように位置を制約

【技術制約】
- 外部ライブラリは使わない(Canvas + 標準DOMイベントのみ)
- 既存のデザイントークン(--bg-card, --border-bright, --accent など)を活用
- モーダルは body 直下に追加、開閉は class の付け外しで制御
- z-index: 100 程度のフルスクリーンオーバーレイ
- 背景は半透明黒(rgba(0,0,0,0.7))で背後をぼかす(backdrop-filter: blur)
- 既存の resizeImage 関数は捨ててOK、新しい cropImage 関数に置き換える

【UI構成】
モーダルの中身:
┌────────────────────────────────────┐
│ [×]                       Crop Image│
│                                    │
│   [大きい画像プレビュー(正方形)]    │
│        ┌──円形クロップガイド──┐     │
│        │                    │     │
│        │   (ここがクロップ範囲)│     │
│        │                    │     │
│        └──────────────────┘     │
│                                    │
│   ズーム: [─────●─────] 1.5x      │
│                                    │
│        [キャンセル]   [確定]        │
└────────────────────────────────────┘

【実装前に必ず確認すること】
- 提案する処理フロー(クロップ計算の数式 + イベント設計)を簡潔に説明してください
- 大きな変更になるので、いきなりコードを書かず、設計案を先に出してください
- 既存の avatar 関連コード(avatarEl.addEventListener('click', ...) など)との
  整合性をどう取るかも説明してください
```

### 完了後にやること
- 様々なサイズの画像でテスト(横長・縦長・正方形・小さい画像)
- スマホでも動作確認できれば理想(ngrokなどでLAN内公開してスマホからアクセス)
- コミット例: `feat: add image crop modal with zoom and drag`

---

## ▶ タスク5: シェア用画像生成(Player Card)

```
タスク5: Phase 1.5 最大の機能、シェア用画像生成(Player Card)を実装します。

【概要】
ユーザーが「Share」ボタンを押すと、現在の自分のプロフィールを 1200×630 px の PNG 画像に
レンダリングして、ブラウザでダウンロードできるようにする。

これがプロダクトのMVP的な心臓部です。

【追加するUI】
- ヒーローセクションのプロフィール行に「Share as Card」というボタンを追加
- ボタンの位置: profile-meta(est. 2003 / N titles logged / JP)の下、または横
- スタイル: 既存の submit-btn と同じトーン(背景 --accent / 文字 --bg-base)
- ボタンを押すと、新しいモーダル(card-preview-modal)が開き、生成された画像の
  プレビューが表示される
- モーダル内に「Download PNG」と「Close」ボタン

【生成する画像のレイアウト】
サイズ: 1200×630 px
背景: --bg-base 〜 --bg-card の縦グラデーション + 微かな radial-gradient のグロー
(現状のサイト背景と同じ雰囲気)

レイアウト案:

┌────────────────────────────────────────────────┐
│                                                │
│  ◯[アバター120px] @yourname                    │ ← 上段:アバター(円形)+ユーザー名
│                                                │
│  The Deep Diver                                │ ← アーキタイプ(英語・主役・最大)
│  深淵潜行型                                      │ ← アーキタイプ(日本語・サブ)
│                                                │
│  "1本のゲームに人生を預ける人。"                  │ ← アーキタイプの説明1行(短縮版)
│                                                │
│  ─── Favorites ───────────────────             │ ← 区切り
│  Hollow Knight · ELDEN RING · Outer Wilds      │ ← TOP3タイトル(評価高い順)
│                                                │
│  324h · 12 Titles · ★4.6                       │ ← 統計1行(モノスペース)
│                                                │
│  freakdex.app/u/yourname                       │ ← URL(右下、小さめ)
└────────────────────────────────────────────────┘

【描画仕様】
- アバター:
  - 円形(clip)、120×120 px、左上角から 60px 内側に配置
  - 画像がない場合は --accent 背景の円形 + 頭文字
- ユーザー名:
  - 現状はユーザー名フィールドが無いので、固定で "@player" としておく(将来Phase 2で動的に)
  - フォント: JetBrains Mono, 24px, --text-secondary
- アーキタイプ英語名:
  - フォント: Fraunces serif italic, 88px, --text-primary
  - state.records.length < 3 の場合は "The Beginner" 固定(まだアーキタイプ判定不可)
- アーキタイプ日本語名:
  - フォント: Noto Sans JP, 32px, --accent
- アーキタイプ説明:
  - 各アーキタイプに対応する短い説明(1行・25字以内)を新規追加してください
  - フォント: Noto Sans JP italic, 22px, --text-secondary
- Favorites区切り線:
  - 細い線 + "Favorites" の小さいラベル(JetBrains Mono 14px --text-muted)
- TOP3タイトル:
  - お気に入り度(rating)が高い順、同率ならプレイ時間が長い順、上位3つを抽出
  - 3本未満の場合は持っているぶんだけ表示
  - フォント: Fraunces 28px, --text-primary
  - タイトル間は " · " で区切る
- 統計1行:
  - フォーマット: "{total_hours}h · {total_titles} Titles · ★{avg_rating}"
  - フォント: JetBrains Mono 22px, --text-secondary
- URL:
  - 右下、フォント: JetBrains Mono 16px, --text-muted
  - 固定で "freakdex.app/u/player" (Phase 2 でユーザー名と連動)

【技術仕様】
- HTML5 Canvas (2D context) で直接描画
- 外部ライブラリ不使用(html2canvas 等は使わない、自前実装)
- フォントは Google Fonts を事前ロード(document.fonts.ready で完了を待つ)
- Canvas → toBlob('image/png') → ダウンロードリンク生成 でダウンロード
- ダウンロードファイル名: "freakdex-card-{timestamp}.png"

【実装手順の提案】
かなり大きい機能なので、以下の手順を強く推奨します:
1. まず Share ボタンとモーダルのHTML/CSSを追加(空のCanvasを置く)
2. 背景グラデーションだけ描画する関数を実装、動作確認
3. テキスト要素を1つずつ追加(タイポグラフィ確認)
4. アバター(円形クリップ)を追加
5. 統計・TOP3タイトルを追加
6. 全体の余白・整列を微調整

【実装前に必ず確認すること】
1. 上記レイアウトのワイヤーフレームを再提示(あなたの理解で再描画してOK)
2. 各アーキタイプの「説明1行」を5パターン(25字以内)を考えて提案してください
3. Canvas で日本語フォントを使うときの注意点(フォント読み込み完了の待ち方など)を
   どう実装するか説明してください
4. 段階的な実装手順を、最初の方の3〜4ステップを具体的に提示してください

設計案が固まってから実装に入ってください。
```

### 完了後にやること
- 様々なデータ量で生成テスト(0本/3本/12本/50本など)
- 生成された画像を実際にSNSに投稿してみる(プレビュー確認)
- コミット例: `feat: add player card image generator`

---

# 🏁 Phase 1.5 完了の判定

5つのタスクすべてが終わったら、以下のチェックリストで完成度を確認:

- [ ] 重要バグ2件が修正済み
- [ ] サイト全体が「Freakdex」表記になっている
- [ ] ヘッダー4枚目が Archetype 表示
- [ ] アバター画像アップロード時にクロップモーダルが開く
- [ ] Share as Card ボタンから1200×630のPNGがダウンロードできる
- [ ] ダウンロードしたPNGをSNSに投稿してプレビュー確認
- [ ] すべてのコミットが GitHub に push 済み
- [ ] CLAUDE.md が新しい内容に更新済み

完了したら DevLog にも記録:
- 何が変わったか
- どこで詰まったか
- 次にやりたいこと(Phase 2)

---

# 📌 注意事項

## 各タスク完了後の動作確認は必須

Claude Code は実装が早いですが、エッジケースを見落とすことがあります。
タスク完了後は必ず:

1. Live Server を開いてブラウザで動作確認
2. 既存機能が壊れていないか(ゲーム記録の追加・削除・統計表示など)を確認
3. LocalStorage のデータが破壊されていないか確認(F12 → Application → Local Storage)
4. 問題なければコミット & push、問題あれば Claude Code に修正依頼

## タスクの順番は変えない

タスク1→2→3→4→5の順で実行してください。理由:
- タスク2(ネーミング)を先にやっておくと、タスク3以降の用語が混乱しない
- タスク3(Archetype昇格)はタスク5(Player Card)で使うので先にやる
- タスク4(クロップ)はタスク5と独立だが、両方画像系なので連続でやると効率的

## Claude Code に渡すときの順序

1. 開幕の状況把握プロンプトを渡す
2. Claude Code が現状を理解した返答を出したら、タスク1を渡す
3. タスク1が完了 → 動作確認OK → コミット → タスク2を渡す
4. 以下繰り返し

途中でやめてもOK。Phase 1.5は5つ全部完成しなくても、Player Cardだけ完成すれば
MVPとして十分機能します。
