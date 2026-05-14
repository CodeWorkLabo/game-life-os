# Game Life OS — Claude Code Instructions

## プロジェクト概要

「**ゲーム人生の可視化**」をテーマにしたWebサービスのプロトタイプ。

単なるゲーム管理ツールではなく、「その人がどんなゲーマーなのか」「どんなゲーム人生を歩んできたのか」を、プレイ履歴・感情・思い出から可視化することを目指す。

サービスの一言: **「あなたのゲーム人生を、1ページに。」**

## コアコンセプト

- **データではなく記憶**: Steamには「プレイ時間」「実績」はあるが、「なぜ好きだったか」「どんな思い出があるか」は存在しない。そこを記録するサービス。
- **ゲーム管理ではなくゲーム人生の可視化**: 機能追加を迷ったときの判断基準。

## 現在のフェーズ

**Phase 1: 超小型MVP**

- 技術スタック: HTML + CSS + JavaScript (1ファイル)
- データ永続化: LocalStorage のみ
- React / Next.js / DB は **まだ導入しない**
- 目的: Web開発の基本フロー習得 と 完成体験

## ファイル構成

```
C:\Projects\
├── index.html        # メインのプロトタイプ(現在は1ファイル完結)
├── package.json
├── README.md
└── CLAUDE.md         # このファイル
```

`index.html` 内部の論理構造:

- **Storage Layer**: `loadRecords` / `saveRecords` / `loadProfile` / `saveProfile`
- **State**: `state = { records, profile, rating, emotions }`
- **Render**: `renderStats` / `renderLibrary` / `renderAIProfile` / `renderWrapped` / `renderAvatar`
- **Form handlers**: 評価 / 感情タグ / アバター画像
- **Demo seed**: 初回起動時にサンプル4本(Hollow Knight / Outer Wilds / Stardew Valley / ELDEN RING)を投入

## LocalStorageキー

- `gamelife.records.v1` — ゲーム記録の配列
- `gamelife.profile.v1` — `{ avatarDataUrl }` プロフィール画像(Base64)
- `gamelife.records.v1.seeded` — デモデータ投入済みフラグ

## 実装済みの機能

- ゲーム記録の追加 / 削除 / 一覧表示
- ★1〜5の評価 + 感情タグ複数選択(9種類)
- プロフィール画像アップロード(256×256にCanvas自動リサイズ → Base64保存)
- ダミーAI分析(プレイ時間と感情タグから5パターンのアーキタイプ判定)
- 2026 Wrapped セクション(Most Played / Most Loved / Defining Emotion)

## 将来のロードマップ(Claude Codeへの参考情報)

| Phase | 内容 |
|-------|------|
| 1 (現在) | 超小型MVP(完了) |
| 2 | UI改善、データのエクスポート/インポート |
| 3 | Next.js + TypeScript化 |
| 4 | Supabase導入(Auth + DB) |
| 5 | Steam Web API連携 |
| 6 | 実AI分析(Anthropic API) |
| 7 | 公開プロフィールページ(gamelife.app/username) |
| 8 | Wrapped 共有可能化 |

## 開発ルール(Claude Codeへの依頼)

### やってほしいこと

- 変更前に **必ず影響範囲を説明** してから手を動かす
- 大きい変更の場合は **複数案を提示** してから選ばせる
- CSS変数 (`:root` の `--bg-base` など) を活用して一貫性を保つ
- 初心者向けに **コメントを丁寧に** 残す
- ファイルを分割するときは事前に相談する

### やらないでほしいこと

- Phase 1の段階で React / TypeScript / npm パッケージ追加 を提案しない
- 一度に大量の変更をしない(小さくコミットできる単位で)
- 既存のデザイントークン(色・フォント)を勝手に変えない
- LocalStorageキーの名前を変えない(`.v1` で固定)

### コーディング規約

- インデント: 2スペース
- セミコロン: あり
- 文字列: シングルクォート優先
- 関数定義: `function` 宣言 or アロー関数、どちらでも可
- 日本語コメントOK

## 開発フロー

1. 変更要件をユーザーと擦り合わせる
2. 必要なら設計案を提示
3. コード修正
4. ユーザーがブラウザで動作確認(Live Server経由でホットリロード)
5. OKならGitコミット(コミットメッセージは Conventional Commits 風に)

## コミットメッセージのフォーマット

```
feat: 新機能
fix: バグ修正
style: 見た目だけの変更
refactor: 動作は変えないコード整理
docs: ドキュメント
chore: その他
```

例: `feat: add tag filter to library section`

## 重要な思想(これだけは忘れないでほしい)

機能追加を迷ったとき、毎回この問いに戻る:

> **「これは"ゲーム管理"の機能? それとも"ゲーム人生の可視化"の機能?」**

前者ならカット、後者なら採用。これだけでスコープが暴走しない。
