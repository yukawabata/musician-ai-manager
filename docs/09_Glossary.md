# 09_Glossary

# Purpose

本ドキュメントは、本プロジェクト内で使用する用語を統一するための用語集である。

全てのドキュメント・設計・実装では、本ドキュメントの定義を優先する。

同じ意味を持つ別名の使用は避け、一つの概念に対して一つの名称のみを使用する。

---

# Core Concepts

## Organization

### Definition

仕事を管理する単位。

個人事業・法人・任意団体などを表す。

MVPでは1ユーザーにつき1Organizationのみ利用可能とするが、データ構造は複数Organizationへ拡張可能な設計とする。

### Examples

- U-Studio
- 川端結
- 株式会社○○

### Notes

- Projectは必ず1つのOrganizationに属する。
- 将来的にはOrganizationごとにメンバーを招待できる。

---

## Business Profile

### Definition

Organization自身の情報。

書類作成時の発行元情報として利用する。

### Examples

- 名称
- 住所
- 電話番号
- メールアドレス
- インボイス登録番号
- 振込口座

### Notes

Document作成時はBusiness Profileを初期値として使用する。

---

## Contact

### Definition

仕事上の相手を表すデータ。

個人・会社・団体などを管理する。

### Examples

- 山田太郎
- 株式会社○○
- ○○吹奏楽団

### Notes

- Conversationとは異なる概念。
- 1つのContactは複数のConversation Groupを持つことができる。
- 1つのContactは複数のProjectを持つことができる。

---

## Conversation Group

### Definition

ユーザーがHome画面で閲覧する会話の単位。

複数のConversation Sourceをまとめて管理するための表示単位。

### Examples

株式会社○○
制作担当

山田太郎

### Notes

Conversation Groupはユーザーが自由に作成・編集・統合・解除できる。

---

## Conversation Source

### Definition

各サービスが持つ実際の会話データ。

### Examples

- LINE
- Gmail
- Instagram
- Facebook Messenger
- X DM

### Notes

Conversation Sourceは必ず1つのConversation Groupに属する。

---

## Message

### Definition

Conversation Source内の個々のメッセージ。

### Notes

AIはMessageを解析対象とする。

---

## Project

### Definition

仕事・案件を管理する単位。

Conversation Groupから作成される。

### Examples

- 定期演奏会委嘱作品
- レコーディング
- 編曲依頼

### Notes

Projectには以下が紐付く。

- Contact
- Task
- Accounting
- Documents

---

## Task

### Definition

Project内で管理する作業。

### Examples

- 見積書作成
- 編曲開始
- 納品
- 請求書送付

---

## Accounting

### Definition

Project単位の収支管理。

### Examples

- 売上
- 外注費
- 交通費
- 消耗品費
- その他経費

---

## Document

### Definition

Projectに紐付く書類。

### Examples

- 見積書
- 発注書
- 納品書
- 請求書
- 領収書

---

## Document Snapshot

### Definition

書類発行時点の情報を保存したデータ。

### Notes

Business ProfileやContact情報が変更されても、発行済み書類は変更されない。

---

## Attachment

### Definition

Conversation・Project・Documentなどに添付されるファイル。

### Examples

- PDF
- Word
- Excel
- 画像
- 音声
- 楽譜

---

## Label

### Definition

Conversation Groupへ付与するユーザー定義のタグ。

### Examples

- 重要
- 要返信
- 作曲
- 学校
- 外注

### Notes

AIが自動変更することはない。

---

# AI Concepts

## AI Suggestion

### Definition

AIがユーザーへ提案を行う機能。

### Notes

提案のみを行い、自動実行は行わない。

---

## AI Classification

### Definition

ConversationやProjectを分類するAI機能。

### Examples

- 案件候補
- 仕事
- プライベート
- 要返信

---

## AI Priority

### Definition

Conversationの優先順位を推定する機能。

### Notes

表示順を補助するための情報であり、ユーザーは自由に変更できる。

---

## AI Extraction

### Definition

Conversationから案件情報や書類情報を抽出する機能。

### Examples

- 案件名
- 納期
- 金額
- クライアント名

---

# Rules

## Naming Rules

- 一つの概念には一つの名称のみを使用する。
- ドキュメント・コード・UIは同一名称を使用する。

---

## Data Rules

- Projectは必ず1つのOrganizationに属する。
- Projectは必ず1つ以上のContactに紐付く。
- Conversation Sourceは必ずConversation Groupに属する。
- Documentは必ずProjectに属する。
- 発行済みDocumentはSnapshotを保持する。

---

## AI Rules

- AIは提案のみを行う。
- AIはユーザーの明示的な承認なしにデータを変更しない。
- AIは削除・送信・発行などの重要操作を実行しない。
- AIの判断はいつでもユーザーが変更できる。

---

## MVP Rules

MVPでは以下を対象とする。

- 1 User
- 1 Organization
- Organizationのメンバー機能なし
- 権限管理なし

将来的に以下へ拡張可能とする。

- 複数Organization
- メンバー招待
- 権限管理
- チーム共同作業
