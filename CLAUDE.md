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
