# Knowledge Codex

制作中に得た知識・気づき・解決策を登録し、AIに整理・補足させて、自分専用の制作知識データベースとして蓄積するクリエイター向けアプリ。

- 公開URL: https://kireibeya-nakamura.github.io/knowledge-codex/
- 方針: **便利さ最優先、その上にサイバーUI**（黒〜濃紺ベース＋シアン発光のホログラムパネル）
- 設計の全体像は [DESIGN.md](DESIGN.md) を参照（データ形式・画面構成・MVPフェーズ）
- 変更履歴は [CHANGELOG.md](CHANGELOG.md) に必ず記録する（引き継ぎ用・必読）

## 構成

- `index.html` — アプリ本体（単一HTML、ビルドなし、GitHub Pagesで直接配信）
- データはブラウザのIndexedDBに保存。バックアップとUE版への供給はJSONエクスポートで行う

## 作業フロー

1. 編集
2. CHANGELOG.md に記録
3. commit & push
4. 携帯で https://kireibeya-nakamura.github.io/knowledge-codex/?v=N を開いて確認（キャッシュ回避のためNを毎回上げる）

ローカルプレビューは不可前提（他プロジェクトと同様）。検証は携帯のGitHub Pagesで行う。
