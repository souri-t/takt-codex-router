# TAKT-inspired Codex Router

Codex のソフトウェア作業を難易度とリスクで分類し、対応するカスタムサブエージェントへ委譲するためのプロジェクト設定です。`takt-router` Skill と `takt_low`、`takt_medium`、`takt_high` の3つのロールを含みます。

このパッケージは TAKT のルーティングエンジンではありません。Skill が作業を分類して、現在の Codex セッションから該当するサブエージェントへ依頼します。委譲先は親セッションのモデル、権限、サンドボックス、承認設定、MCP ツールを引き継ぎます。

## 含まれるファイル

- `.agents/skills/takt-router/SKILL.md` — 分類と委譲の手順
- `.agents/skills/takt-router/routing-policy.md` — 難易度・リスクの判定基準とフォールバック
- `.codex/config.toml` — プロジェクト内でサブエージェントを有効にし、3つのロールを登録
- `.codex/agents/takt_low.toml` — 小さく明確で、元に戻しやすい作業
- `.codex/agents/takt_medium.toml` — 複数手順や複数箇所に及ぶ作業
- `.codex/agents/takt_high.toml` — セキュリティ、永続データ、本番環境など高リスクの作業
- `INSTALL.md` — 対象リポジトリへの導入と設定確認の手順

## インストール

`INSTALL.md` の手順に従ってください。概要は次のとおりです。

1. `.agents/` と `.codex/` を対象リポジトリのルートへコピーします。
2. 同名ディレクトリがある場合は統合し、既存の `.codex/config.toml` は上書きせず設定をマージします。
3. 対象リポジトリを Codex で開き、求められた場合は信頼します。
4. Skill やロールが表示されない場合は、プロジェクトを再読み込みします。

## 使い方

ソフトウェア作業を依頼するとき、次のように指定します。

> Use `$takt-router` to classify this task and delegate it to the matching custom subagent.

Skill はまず `routing-policy.md` を読み、最も高いリスクまたは難易度に合わせてロールを1つ選びます。リスクが不明な場合は上位のロールを選びます。計画や説明だけの依頼では、委譲せずに推奨ロールを示す場合があります。

## 動作要件と設定

- プロジェクト Skill とカスタムサブエージェントに対応した Codex クライアントが必要です。
- プロジェクトを信頼するまで、Codex は `.codex/config.toml` を読み込みません。
- サブエージェントは親セッションの実行設定を引き継ぎます。この設定一式は権限や承認制限を変更しません。
- エージェント設定に `model` は指定していません。親セッションのモデルを引き継ぎます。個別のモデルを指定する場合は、利用可能なモデル ID を確認してから各 `.codex/agents/takt_*.toml` に設定してください。
- `model_reasoning_effort` が利用中のモデルで受け付けられない場合は、その設定行を削除して既定値を使ってください。
- `.codex/config.toml` はサブエージェントを有効にし、同時スレッド数の上限を3に設定します。

## 動作確認

Codex 上で `takt_low`、`takt_medium`、`takt_high` の3ロールと `takt-router` Skill が認識されることを確認してください。導入直後は、文書の軽微な修正など影響の小さい依頼で分類、委譲、結果確認の流れを試してください。詳細は `INSTALL.md` を参照してください。

## ライセンスと位置づけ

このリポジトリは TAKT に着想を得た Codex 用の設定例です。TAKT の公式ルーティングエンジンではありません。Codex の設定や対応状況は変わることがあるため、モデル ID などを固定する場合は最新の公式ドキュメントを確認してください。
