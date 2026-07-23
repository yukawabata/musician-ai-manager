# AI Development Rules

このドキュメントは、Musician AI Managerの開発に参加するAIおよび開発者が守る共通ルールを定める。

## 1. 役割

### Product Owners

川端結および山本しょうりゅうを、同等の権限を持つProduct Ownerとする。

Product Ownerは、以下の最終決定を行う。

* プロダクトの方向性
* 機能の採否
* 優先順位
* デザイン
* リリース判断
* 仕様変更

### ChatGPT

ChatGPTは、プロダクト設計および技術設計を支援する。

主な担当は以下とする。

* 要件整理
* 仕様書作成
* システム設計
* データベース設計
* AI機能設計
* ロードマップ作成
* Issueの整理
* 実装内容のレビュー
* Claudeへ渡す実装指示の作成

### Claude

Claudeは、主に実装を担当する。

主な担当は以下とする。

* Flutterアプリの実装
* Supabaseとの接続
* データベース実装
* API連携
* UI実装
* テスト作成
* バグ修正
* リファクタリング
* 技術ドキュメント更新

## 2. Single Source of Truth

プロジェクトの正式な仕様は、GitHubリポジトリ内の`docs/`に記載された内容とする。

Discord、ChatGPT、Claude、口頭での会話だけで決まった内容は、正式な仕様とはみなさない。

重要な決定は、必ず以下のいずれかへ反映する。

* `docs/`
* GitHub Issue
* Pull Request
* `docs/08_Decisions.md`

## 3. 実装前の確認

実装を開始する前に、必ず以下を確認する。

1. `README.md`
2. `docs/00_ProjectOverview.md`
3. `docs/01_ProductConcept.md`
4. `docs/02_MVP.md`
5. 実装対象に関係する仕様書
6. 対象となるGitHub Issue

仕様書とIssueの内容が矛盾している場合は、実装を停止し、Product Ownerへ確認する。

## 4. 仕様の取り扱い

仕様が存在しない機能を推測して実装してはならない。

実装中に仕様変更が必要だと判断した場合は、以下の手順を取る。

1. 問題点を整理する
2. 変更案を提示する
3. GitHub IssueまたはPull Requestへ記録する
4. Product Ownerの承認を得る
5. 仕様書を更新する
6. 実装する

AIが独断で仕様を変更してはならない。

## 5. MVP優先

開発ではMVPの完成を最優先する。

以下の実装は避ける。

* MVPに記載されていない機能
* 将来使う可能性だけを理由にした過剰な設計
* 不要な抽象化
* 必要以上に複雑なアーキテクチャ
* Product Ownerが理解できない仕組み

## 6. コード品質

コードは、短さよりも可読性と保守性を優先する。

以下を守る。

* 役割が明確な命名を使用する
* 1つのファイルに責務を集中させすぎない
* 重複コードを放置しない
* 不要な依存パッケージを追加しない
* エラー処理を省略しない
* 秘密情報をコードへ直接記載しない
* APIキーをGitHubへコミットしない
* 必要な箇所にテストを作成する

## 7. セキュリティ

以下の情報をリポジトリへ直接保存してはならない。

* APIキー
* パスワード
* Supabase Service Role Key
* AppleやGoogleの認証情報
* Meta APIのアクセストークン
* ユーザーの個人情報
* 実際のメールやメッセージ内容
* 決済サービスの秘密鍵

環境変数の例が必要な場合は、実際の値を入れず`.env.example`を使用する。

## 8. GitおよびGitHub

作業は原則としてIssue単位で行う。

コミットは、内容が分かる単位に分割する。

コミットメッセージの例：

* `feat: add project creation screen`
* `fix: correct project deadline validation`
* `docs: update MVP requirements`
* `test: add project repository tests`
* `refactor: simplify notification parser`

大きな変更は、原則としてPull Requestを作成し、レビュー後に`main`へ統合する。

## 9. Pull Request

Pull Requestには、最低限以下を記載する。

* 何を変更したか
* なぜ変更したか
* 関連するIssue
* 動作確認方法
* スクリーンショット
* 未解決事項
* 仕様書への影響

## 10. 不明点

情報が不足している場合、AIはもっともらしい内容を作って補ってはならない。

軽微な実装上の判断は、一般的なベストプラクティスに従ってよい。

以下に関わる判断は、Product Ownerへ確認する。

* ユーザー体験
* 課金
* 個人情報
* 外部サービス連携
* データの保存・削除
* AIの判断基準
* 利用規約
* 重要な画面構成
* MVPの範囲

## 11. ドキュメント更新

実装によって仕様が変わった場合、コードだけでなく関連ドキュメントも同じPull Request内で更新する。

実装とドキュメントが異なる状態を放置してはならない。
