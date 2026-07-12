# Knowledge Codex — 引き継ぎ文書（2026-07-10 / v14時点）

新しいチャット・別のAI・未来の自分がここから開発を再開するための文書。
**判断基準は `design-director/SKILL.md`、仕様は `DESIGN.md`、履歴は `CHANGELOG.md`。本書は全体地図。**

## 1. これは何か
制作中に得た知識をAIに整理させ、自分専用の知識DBに育てるクリエイター向けアプリ。
作者は絵描き出身（CG/コーディング学習中）。転職ポートフォリオ兼実用ツール。
哲学＝**便利さ最優先、サイバー装飾はその上**。ゲーム化せずコレクション感を出す。

## 2. 場所と作業フロー
- 本体: `C:\Users\Nakam\Documents\KnowledgeCodex\index.html`（**ビルドなし・ライブラリ依存ゼロ**。v24から効果音のみ `sounds/*.wav` 6ファイル＝Kenney CC0）
- リポジトリ: github.com/kireibeya-nakamura/knowledge-codex（public）
- 公開: https://kireibeya-nakamura.github.io/knowledge-codex/ （確認は `?v=N` でキャッシュ回避）
- フロー: **編集 → CHANGELOG追記 → 検証 → commit/push → 携帯実機で確認**（作者は必ず実機で見る。縦横両方）
- ローカル検証: `C:\.claude\launch.json` の `codex-static`（port 8127、Claude Preview用）。
  **preview_resizeのモバイルエミュレーションは不安定** → レスポンシブは実機確認
- デプロイ: GitHub Actions（`.github/workflows/pages.yml`、push→1〜2分で反映）。
  **失敗時はrerun禁止**（アーティファクト二重で必ず失敗）→ `POST .../actions/workflows/pages.yml/dispatches` で新規実行
- git: identityはリポジトリローカル設定済み。remoteはPAT埋め込みURL（majongと同方式、workflowスコープ有）

## 3. データモデル（すべての土台）
IndexedDB `knowledge-codex` v2: `records`（知識）+ `files`（画像blob、canvas圧縮1280px JPEG）。
レコードスキーマは DESIGN.md 参照。要点:
- ID `KX-0001` 連番（コレクション感の核）。`type`: solution/insight/term/recipe/prompt/reference
- status 4つのみ: captured（未整理・琥珀）→ enriched（AI整理済・青）→ applied（使用済・緑）、+archived。他はフラグ
- `ai.*` はユーザー原文と分離保持。追記は `appendLog[]`（上書きしない）。使用履歴 `usedIn[]`
- バックアップ: 設定→JSONエクスポート（画像base64同梱）/インポート。UE版（未着手）はこのJSONを読む想定

## 4. 画面とルート（ハッシュSPA）
`#/`ホーム(3Dデスク) / `#/capture`登録 / `#/list`一覧(検索・フィルタ・カード/表切替) / `#/detail/ID` /
`#/ai/ID`AI整理 / `#/cards`カードブック / `#/card/ID`カード単体(回転) / `#/terms`用語集 / `#/settings`
- ホームだけCSS 3D（perspective+rotateY、パララックス=マウス/ジャイロ+アイドルスウェイ）。**作業画面は正面フラット**
- 戻る: HUD右上の `#hud-back`（`body.view-open` で stats/⚙ と入れ替わり表示）
- 効果音: `beep(kind)` 6種。v24から**Kenney CC0サンプル再生**（`sounds/*.wav`、割当はsounds/LICENSE.txt参照）。`kx-sound`でOFF可。合成時代の教義（音程スイープ禁止）は再合成する場合のみ適用

## 5. AI整理（コア機能）
- 設定にAPIキー（localStorage `kx-api-key`）とモデル選択（`kx-ai-model`）。**ユーザーはまだキー未設定（保留中）**
- キーあり: fetchで api.anthropic.com 直叩き（`anthropic-dangerous-direct-browser-access` ヘッダ）。
  **structured outputs（output_config.format + AI_SCHEMA）で返答形式を強制**
- キーなし: プロンプトコピー→返答JSON貼り付けのフォールバック
- 提案は点線パネル→チップをタップで採用→captured→enrichedへ。既存タグをプロンプトに含めて表記ゆれ防止

## 6. localStorageキー
`kx-api-key` / `kx-ai-model` / `kx-sound` / `kx-list-view` / `kx-last-cat`

## 7. 次の候補（未着手）
1. **APIキー設定→実キーでAI整理の実地確認**（保留中、最優先候補）
2. **Phase 4: 関連知識ネットワーク**（ai.relatedIds を星図表示。ビジュアルの目玉）
3. **UE版ビューア**（エクスポートJSONを読む閲覧専用3Dデモ、ポートフォリオ用）

## 8. 引き継ぎ時の注意
- 変更は小さく刻み、毎回CHANGELOGに書き、実機確認を挟む（作者の好み）
- デザイン判断に迷ったら `design-director/SKILL.md` の基準とDesign Logに従い、決定したら**ログに1行追記**
- 既存データを壊さない（IndexedDBスキーマ変更は必ずマイグレーション）
