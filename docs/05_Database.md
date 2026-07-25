# 05_Database

# Purpose

本ドキュメントは、本サービスにおけるデータ構造・エンティティ・リレーション・保存ルールを定義する。

本ドキュメントでは、特定のデータベース製品やSQL構文には依存せず、プロダクトとして保持すべき情報と、その責務を明確にする。

物理的なテーブル設計・型定義・インデックス・API仕様は、本ドキュメントを前提として別途定義する。

---

# Design Philosophy

## Contact First

本サービスの中心データは、ProjectではなくContactである。

仕事は単発の案件だけで成立するものではなく、人・会社・団体との継続的な関係の中で発生する。

一つのContactに対して、複数のConversation Group・Project・Document・Accounting情報を紐付けられる構造とする。

---

## Conversation First

仕事は、会話から始まる。

LINE・Gmail・Instagram・X・FacebookなどのConversation Sourceで発生した会話を整理し、その会話からProjectを作成することを基本フローとする。

ただし、会話を経由せず、ユーザーがProjectを直接作成することも可能とする。

---

## Organization Ownership

すべての業務データは、原則としてOrganizationに属する。

MVPでは1 User・1 Organizationとして実装する。

ただし、将来的な以下の機能へ拡張可能な構造とする。

- 複数Organization
- メンバー招待
- 権限管理
- チーム共同作業
- Organization間の切り替え

---

## One Source of Truth

同一情報を複数の場所で独立管理しない。

一つの情報には、一つの正しい保持場所を持たせる。

各画面は、そのデータを複製するのではなく参照する。

ただし、発行済みDocumentなど、過去の状態を保持する必要がある情報についてはSnapshotを保存する。

---

## Human Controlled

AIはデータの候補・分類・優先順位・入力内容を提案できる。

ただし、ユーザーの明示的な承認なしに以下を行わない。

- Contactの統合
- Conversation Sourceの統合
- Projectの作成
- データの変更
- メッセージの送信
- Documentの発行
- データの削除

---

# Core Data Flow

本サービスにおける、代表的な仕事の流れは以下とする。

```text
External Services

LINE
Gmail
Instagram
X
Facebook
etc.

        │
        ▼

Integration Account

        │
        ▼

Conversation Source

        │
        ▼

Conversation Group

        │
        ├───────────────┐
        ▼               ▼

     Contact          Project

                        │
          ┌─────────────┼─────────────┐
          ▼             ▼             ▼

        Task         Document     Accounting Entry

                                      │
                                      ▼

                                   Archive
```

---

# Core Entity Relationship

```text
User
│
└── Organization
    │
    ├── Business Profile
    │
    ├── Integration Account
    │
    ├── Contact
    │   ├── Contact Channel
    │   ├── Conversation Group
    │   └── Project Contact
    │
    ├── Conversation Group
    │   ├── Conversation Source
    │   │   └── Message
    │   └── Label
    │
    ├── Project
    │   ├── Project Contact
    │   ├── Project Billing Profile
    │   ├── Task
    │   ├── Accounting Entry
    │   ├── Document
    │   └── Attachment
    │
    └── AI Metadata
```

---

# Entity Definitions

# User

## Definition

本サービスを利用する個人アカウント。

認証・個人設定・Organizationとの関係を管理する。

## Main Fields

- id
- display_name
- email
- profile_image
- timezone
- locale
- status
- created_at
- updated_at
- archived_at

## Relations

- 1 Userは、MVPでは1 Organizationを所有する。
- 将来的には複数Organizationへ所属可能とする。

## Notes

UserとBusiness Profileは別の概念とする。

Userはアプリ利用者本人を表し、Business Profileは仕事上の発行元情報を表す。

---

# Organization

## Definition

仕事データを管理する最上位単位。

個人事業・法人・任意団体などを表す。

## Main Fields

- id
- name
- owner_user_id
- status
- default_currency
- timezone
- created_at
- updated_at
- archived_at

## Relations

Organizationは以下を所有する。

- Business Profile
- Contact
- Conversation Group
- Project
- Document
- Accounting Entry
- Integration Account
- Label

## MVP Rules

- 1 Userにつき1 Organization
- Organization追加機能なし
- メンバー招待なし
- 権限管理なし

## Future Expansion

将来的には以下を追加可能とする。

- Organization Member
- Role
- Permission
- Invitation
- 複数Organization切り替え

---

# Business Profile

## Definition

Organization自身の事業者情報。

Documentの発行元情報や、振込先情報の初期値として利用する。

## Main Fields

- id
- organization_id
- display_name
- legal_name
- representative_name
- postal_code
- address
- phone
- email
- website
- invoice_registration_number
- tax_settings
- default_payment_terms
- default_bank_account_id
- logo_attachment_id
- created_at
- updated_at

## Relations

- 1 Organizationは1つ以上のBusiness Profileを持てる設計とする。
- MVPでは1 Organizationにつき1 Business Profileを基本とする。
- Projectは使用するBusiness Profileを指定できる。
- Document作成時にBusiness Profileを参照する。

## Notes

将来的に、同一Organization内で複数の発行名義や銀行口座を使い分けられるようにする。

---

# Bank Account

## Definition

OrganizationがDocumentの振込先として利用する銀行口座。

## Main Fields

- id
- organization_id
- bank_name
- branch_name
- account_type
- account_number
- account_holder
- is_default
- created_at
- updated_at
- archived_at

## Relations

- Business Profileのデフォルト振込先として設定できる。
- Projectごとに上書きできる。
- Document発行時にSnapshotへ保存する。

---

# Contact

## Definition

仕事上の相手を表すデータ。

個人・会社・団体・部署などを管理する。

## Contact Types

- person
- company
- organization
- department
- other

## Main Fields

- id
- organization_id
- contact_type
- display_name
- legal_name
- company_name
- department
- title
- representative_name
- postal_code
- address
- phone
- email
- website
- tax_registration_number
- notes
- status
- created_at
- updated_at
- archived_at

## Relations

- 1 Contactは複数のContact Channelを持てる。
- 1 Contactは複数のConversation Groupに紐付けられる。
- 1 Contactは複数のProjectに紐付けられる。
- 1 Projectは複数のContactに紐付けられる。

## Notes

Contactは、必ずしもConversation Groupと1対1ではない。

例：

```text
Contact：株式会社○○

Conversation Group：
- 制作担当
- 経理担当
- 広報担当
```

---

# Contact Channel

## Definition

Contactが持つ連絡先情報。

Conversation Sourceそのものではなく、Contact Profile内の連絡手段を表す。

## Channel Types

- email
- phone
- LINE
- Instagram
- X
- Facebook
- website
- other

## Main Fields

- id
- contact_id
- channel_type
- value
- display_name
- is_primary
- verified_status
- created_at
- updated_at
- archived_at

## Notes

Contact Channelは、プロフィール上の連絡先情報である。

外部サービスから取得された実際のトークやスレッドはConversation Sourceとして管理する。

---

# Conversation Group

## Definition

Home画面に表示される会話の単位。

複数のConversation Sourceを統合して閲覧するための論理的な表示グループ。

## Main Fields

- id
- organization_id
- display_name
- conversation_type
- status
- priority
- last_message_at
- unread_count
- is_pinned
- created_by
- created_at
- updated_at
- archived_at

## Conversation Types

- direct
- group
- company
- team
- other

## Relations

- 1 Conversation Groupは複数のConversation Sourceを持てる。
- 1 Conversation Groupは複数のContactに紐付けられる。
- 1 Conversation Groupから複数のProjectを作成できる。
- 1 Conversation Groupは複数のLabelを持てる。

## Rules

- Home画面にはConversation SourceではなくConversation Groupを表示する。
- Conversation Groupの統合・解除はユーザーが行う。
- AIは統合候補を提案できる。
- AIは自動統合しない。

## Notes

Conversation Groupは人を表すものではない。

グループチャット・部署・複数人での会話を含められる。

---

# Conversation Group Contact

## Definition

Conversation GroupとContactの多対多関係を管理する中間データ。

## Main Fields

- id
- conversation_group_id
- contact_id
- role
- is_primary
- created_at
- updated_at

## Role Examples

- client
- person_in_charge
- accounting
- participant
- external_partner
- other

---

# Conversation Source

## Definition

外部サービス上に存在する実際の会話・スレッド・DM・チャット。

## Source Types

- Gmail Thread
- LINE Talk
- Instagram DM
- X DM
- Facebook Messenger
- Manual Conversation
- other

## Main Fields

- id
- organization_id
- conversation_group_id
- integration_account_id
- external_source_id
- source_type
- source_name
- external_url
- status
- last_synced_at
- last_message_at
- created_at
- updated_at
- archived_at

## Relations

- Conversation Sourceは必ず1つのConversation Groupに属する。
- Conversation Sourceは複数のMessageを持つ。
- Conversation Sourceは1つのIntegration Accountに属する。

## Rules

- 外部サービス上の識別子を保持する。
- 同一外部会話を重複登録しない。
- Conversation Groupから解除されてもMessageは削除しない。
- 外部サービスから削除された場合も、保存済みデータの扱いは削除ポリシーに従う。

---

# Message

## Definition

Conversation Source内の個々のメッセージ。

## Main Fields

- id
- conversation_source_id
- external_message_id
- sender_type
- sender_external_id
- sender_display_name
- direction
- body_text
- sent_at
- received_at
- read_status
- reply_to_message_id
- status
- created_at
- updated_at
- deleted_at

## Direction

- inbound
- outbound
- system

## Relations

- Messageは1つのConversation Sourceに属する。
- Messageは複数のAttachmentを持てる。
- AI Metadataを持てる。

## AI Analysis Examples

- 案件候補
- 要返信
- 納期候補
- 金額候補
- 依頼内容候補
- 感情・緊急度
- Project候補

## Rules

AI解析結果はMessage本体を変更せず、AI Metadataとして別管理する。

---

# Label

## Definition

Conversation Groupをユーザーが分類するためのタグ。

## Main Fields

- id
- organization_id
- name
- description
- display_order
- status
- created_at
- updated_at
- archived_at

## Relations

Conversation Groupとは多対多で紐付く。

## Rules

- 1 Conversation Groupに複数Labelを付与できる。
- Label名はユーザーが自由に編集できる。
- AIはユーザーLabelを自動変更しない。
- AI分類はLabelとは別データとして保持する。

---

# Conversation Group Label

## Definition

Conversation GroupとLabelの多対多関係を管理する中間データ。

## Main Fields

- id
- conversation_group_id
- label_id
- created_at

---

# Integration Account

## Definition

Gmail・LINE・Instagramなどの外部サービス連携アカウント。

## Main Fields

- id
- organization_id
- provider
- external_account_id
- display_name
- account_email
- connection_status
- permission_scope
- token_reference
- last_synced_at
- created_at
- updated_at
- disconnected_at

## Rules

- 認証トークン本体を通常データとして直接保存しない。
- 安全な秘密情報管理領域に保存する。
- 連携解除後も、既に取得済みのConversation Source・Messageは即時削除しない。
- 利用規約・API制約・ユーザーの削除要求に従う。

---

# Project

## Definition

仕事・案件を管理する単位。

Conversation GroupまたはContactを起点として作成できる。

## Main Fields

- id
- organization_id
- business_profile_id
- source_conversation_group_id
- name
- description
- project_type
- status
- priority
- start_date
- due_date
- event_date
- completed_at
- currency
- expected_revenue
- confirmed_revenue
- notes
- created_by
- created_at
- updated_at
- archived_at

## Project Status

- draft
- active
- waiting
- on_hold
- completed
- cancelled
- archived

## Relations

Projectは以下を持つ。

- Project Contact
- Project Billing Profile
- Task
- Accounting Entry
- Document
- Attachment

## Rules

- Projectは必ず1つのOrganizationに属する。
- Projectは1つ以上のContactに紐付けることを基本とする。
- Conversation Groupから作成された場合、元Conversation Groupを保持する。
- Conversation GroupからProjectを削除しても、元のConversation Groupは削除しない。
- Project完了後も関連データは保持する。

---

# Project Contact

## Definition

ProjectとContactの多対多関係を管理する中間データ。

## Main Fields

- id
- project_id
- contact_id
- role
- is_primary
- created_at
- updated_at

## Role Examples

- client
- billing_recipient
- person_in_charge
- performer
- contractor
- supplier
- collaborator
- other

## Notes

1つのProjectに、依頼主・担当者・請求先・外注先など複数のContactを紐付けられる。

---

# Project Billing Profile

## Definition

Project固有の請求先・送付先・支払条件を管理するデータ。

Contactの情報を初期値として作成し、案件ごとに上書きできる。

## Main Fields

- id
- project_id
- billing_contact_id
- recipient_name
- company_name
- department
- postal_code
- address
- email
- phone
- tax_registration_number
- payment_terms
- closing_date_rule
- payment_due_rule
- bank_account_id
- notes
- created_at
- updated_at

## Rules

- Contact Profileの変更と独立して編集できる。
- Document作成時の初期値として利用する。
- 発行済みDocumentにはSnapshotとしてコピーする。
- Project Billing Profile変更後も、過去のDocumentは変更しない。

---

# Task

## Definition

Project内で管理する具体的な作業。

## Main Fields

- id
- project_id
- title
- description
- status
- priority
- due_date
- completed_at
- assigned_user_id
- source_message_id
- display_order
- created_at
- updated_at
- archived_at

## Task Status

- todo
- in_progress
- waiting
- completed
- cancelled
- archived

## Relations

- Taskは必ず1つのProjectに属する。
- Messageから作成された場合、元Messageを参照できる。
- 将来的にはサブタスク・依存関係を追加可能とする。

## Rules

- AIはTask候補を提案できる。
- AIはユーザー承認なしにTaskを作成しない。
- Task完了後も履歴を保持する。

---

# Accounting Entry

## Definition

Projectに関係する収入・支出・入出金予定を管理するデータ。

## Entry Types

- revenue
- expense
- outsourcing
- transportation
- equipment
- tax
- adjustment
- other

## Main Fields

- id
- organization_id
- project_id
- contact_id
- document_id
- entry_type
- category
- description
- amount
- tax_amount
- tax_rate
- currency
- transaction_date
- due_date
- payment_status
- payment_method
- account_name
- notes
- created_at
- updated_at
- archived_at

## Payment Status

- planned
- unpaid
- partially_paid
- paid
- overdue
- cancelled

## Relations

- Accounting EntryはOrganizationに属する。
- 原則としてProjectに紐付く。
- 必要に応じてContact・Documentに紐付く。
- Attachmentを持てる。

## Rules

- Project画面内のAccountingと、メインナビゲーションのAccountingは同一データを参照する。
- DocumentからAccounting Entryを作成できる。
- Accounting Entryを変更しても、発行済みDocumentの金額は変更しない。
- 会計データの物理削除は原則行わない。

---

# Document

## Definition

Projectに紐付く業務書類。

## Document Types

- estimate
- purchase_order
- delivery_note
- invoice
- receipt
- other

## Main Fields

- id
- organization_id
- project_id
- document_type
- document_number
- title
- status
- issue_date
- due_date
- currency
- subtotal
- tax_total
- total
- notes
- created_by
- issued_at
- created_at
- updated_at
- archived_at

## Document Status

- draft
- confirmed
- issued
- sent
- paid
- cancelled
- archived

## Relations

- Documentは必ず1つのProjectに属する。
- DocumentはDocument Snapshotを持つ。
- Document Itemを複数持つ。
- Attachmentを持てる。
- Accounting Entryと紐付けられる。

## Rules

- Draft中は元データを参照し、編集可能とする。
- 発行時にSnapshotを生成する。
- 発行後の内容変更は、原則として新しい版または再発行として扱う。
- 発行済みDocumentをContactやBusiness Profileの変更で自動更新しない。

---

# Document Item

## Definition

Document内の明細行。

## Main Fields

- id
- document_id
- name
- description
- quantity
- unit
- unit_price
- tax_rate
- subtotal
- display_order
- created_at
- updated_at

## Rules

- 金額計算はDocument Itemを基準とする。
- Documentの合計値は明細から算出する。
- 発行時にはDocument Snapshotへ保存する。

---

# Document Snapshot

## Definition

Document発行時点の全情報を固定保存したデータ。

## Snapshot Contents

- 発行元情報
- 発行元住所
- 振込口座
- インボイス登録番号
- 宛先情報
- 請求先情報
- Document明細
- 金額
- 税率
- 支払期限
- 備考
- Document番号
- 発行日
- 表示設定
- テンプレート情報

## Main Fields

- id
- document_id
- version
- snapshot_data
- generated_file_attachment_id
- created_at

## Rules

- 発行済みDocumentは必ずSnapshotを持つ。
- Snapshotは原則として変更しない。
- 修正が必要な場合は新しいversionを作成する。
- 元のBusiness Profile・Contact・Project Billing Profileが変更されてもSnapshotは変化しない。

---

# Attachment

## Definition

各データへ添付されるファイル。

## Attachment Types

- image
- pdf
- document
- spreadsheet
- audio
- video
- score
- archive
- other

## Main Fields

- id
- organization_id
- file_name
- original_file_name
- mime_type
- file_size
- storage_key
- checksum
- source
- uploaded_by
- created_at
- archived_at

## Relations

Attachmentは以下へ紐付け可能とする。

- Message
- Contact
- Project
- Task
- Accounting Entry
- Document
- Document Snapshot

## Rules

- ファイル本体とメタデータを分離する。
- 同一ファイルの重複保存を検知できる構造を推奨する。
- 外部サービスから取得した添付ファイルは取得元を保持する。

---

# AI Metadata

## Definition

AIによる解析結果・候補・分類・優先順位を、元データと分離して保存するデータ。

## Main Fields

- id
- organization_id
- target_type
- target_id
- analysis_type
- result_data
- confidence
- model_name
- model_version
- status
- user_feedback
- accepted_at
- rejected_at
- created_at
- updated_at

## Target Examples

- Message
- Conversation Group
- Contact
- Project
- Document
- Task

## Analysis Types

- classification
- priority
- project_candidate
- task_candidate
- contact_merge_candidate
- conversation_merge_candidate
- information_extraction
- reply_suggestion
- document_suggestion

## Status

- pending
- suggested
- accepted
- rejected
- expired

## Rules

- AI Metadataは元データを直接変更しない。
- ユーザーが承認した場合のみ、本データへ反映する。
- AIの結果・使用モデル・承認履歴を追跡可能にする。
- AI結果は再生成可能とし、唯一の正しい業務データとして扱わない。

---

# Status Model

各エンティティは、それぞれの業務状態と保存状態を分けて管理する。

## Business Status

業務上の進行状態。

例：

- Project Status
- Task Status
- Document Status
- Payment Status

## Record Status

データそのものの状態。

共通例：

- active
- archived
- deleted

## Rules

- completedとarchivedを混同しない。
- completedは業務が完了した状態。
- archivedは通常画面から非表示にする保存状態。
- cancelledは業務が中止された状態。
- deletedは削除処理対象となった状態。

---

# Data Ownership

## Organization Ownership

以下のデータは必ずOrganizationに属する。

- Business Profile
- Bank Account
- Contact
- Conversation Group
- Conversation Source
- Project
- Accounting Entry
- Document
- Label
- Integration Account
- Attachment
- AI Metadata

## Project Ownership

以下のデータは原則としてProjectに属する。

- Task
- Project Billing Profile
- Accounting Entry
- Document
- Project Attachment

## Cross-Organization Rules

- 異なるOrganization間で通常データを直接共有しない。
- 将来的なOrganization間共有は、明示的な共有機能として実装する。
- Organization IDによるデータ分離を必須とする。

---

# Archive and Delete Policy

## Basic Policy

物理削除よりArchiveを優先する。

仕事・会計・書類・会話の履歴は、後から参照できることが重要である。

## Archive

Archiveされたデータは通常画面から非表示となるが、データ自体は保持する。

対象例：

- Contact
- Conversation Group
- Project
- Task
- Document
- Accounting Entry
- Label

## Soft Delete

削除操作が必要な場合は、deleted_atなどを用いたSoft Deleteを基本とする。

## Physical Delete

以下の場合のみ検討する。

- ユーザーから明示的な完全削除要求がある
- 法令・規約上、削除が必要
- 保存する合理的理由がない一時データ
- セキュリティ上の理由がある

## Protected Records

以下は安易に削除しない。

- 発行済みDocument
- Document Snapshot
- Accounting Entry
- 送受信済みMessage
- AI承認履歴
- 監査に必要な操作履歴

---

# Audit Log

## Definition

重要操作の履歴を記録するデータ。

## Logged Actions

- Contact統合・解除
- Conversation Source統合・解除
- Project作成・変更
- Document発行・再発行・取消
- Accounting Entry変更
- AI提案の承認・却下
- 外部サービス連携・解除
- Archive・Restore
- 重要データの削除

## Main Fields

- id
- organization_id
- user_id
- action
- target_type
- target_id
- before_data
- after_data
- created_at

## MVP

MVPでは、すべての変更履歴を詳細に保存する必要はない。

ただし、以下は初期段階から記録することを推奨する。

- Document発行
- Document取消
- Accounting変更
- Conversation統合・解除
- AI提案の承認

---

# Data Synchronization

## External Data

Conversation Source・Messageは、外部サービスとの同期状態を保持する。

## Required Fields

- external_id
- provider
- last_synced_at
- sync_status
- sync_error
- source_updated_at

## Rules

- 外部データと内部データを識別できるようにする。
- 同じ外部データを重複保存しない。
- 同期失敗によって既存データを破壊しない。
- 外部サービスで変更された場合の競合ルールを定義する。
- ユーザーが入力した内部データを外部同期で上書きしない。

---

# MVP Scope

MVPで実装する主要エンティティは以下とする。

- User
- Organization
- Business Profile
- Bank Account
- Contact
- Contact Channel
- Conversation Group
- Conversation Group Contact
- Conversation Source
- Message
- Label
- Conversation Group Label
- Integration Account
- Project
- Project Contact
- Project Billing Profile
- Task
- Accounting Entry
- Document
- Document Item
- Document Snapshot
- Attachment
- AI Metadata

---

# MVP Simplifications

MVPでは以下を簡略化する。

## Organization

- 1 User
- 1 Organization
- Organization追加なし
- メンバーなし
- 権限管理なし

## Business Profile

- 原則1つ
- 複数発行名義のUIなし

## Contact

- 個人・会社・団体を同一エンティティで管理する
- 高度なCRM機能は実装しない

## Conversation

- 対応可能な外部サービスから段階的に実装する
- 外部サービス連携が難しい場合は、Gmailまたは手動Conversationから開始可能とする

## Document

- 基本的な5種類を実装する
- 高度なテンプレート編集は将来実装とする

## Accounting

- Project単位の簡易収支管理を中心とする
- 会計ソフト相当の複式簿記機能はMVP対象外とする

## AI

- 分類
- 優先順位
- 案件候補
- 情報抽出
- Task候補
- 返信案
- Document入力補助

AIによる自動実行は実装しない。

---

# Future Extensions

将来的に以下のエンティティ・機能を追加できる設計とする。

## Team

- Organization Member
- Role
- Permission
- Invitation
- Department
- Assigned Member

## CRM

- Contact Relationship
- Company and Person separation
- Sales Stage
- Lead
- Opportunity
- Contact History

## Project Management

- Subtask
- Task Dependency
- Milestone
- Recurring Task
- Calendar Event
- Work Log
- Time Tracking

## Accounting

- Account
- Journal Entry
- Tax Category
- Reconciliation
- Accounting Software Integration
- Bank Integration
- Credit Card Integration

## Document

- Custom Template
- Electronic Signature
- Approval Flow
- Version Comparison
- Scheduled Sending
- Payment Link

## AI

- AI Agent
- Periodic Review
- Follow-up Detection
- Schedule Proposal
- Project Risk Detection
- Cash Flow Forecast
- Organization-level Knowledge

---

# Database Design Rules

実装時は以下のルールを遵守する。

1. すべての主要データに一意のIDを付与する。
2. すべての業務データをOrganization単位で分離する。
3. created_at・updated_atを原則として保持する。
4. Archive対象データにはarchived_atを持たせる。
5. 外部データにはexternal_idとproviderを保持する。
6. 発行済みDocumentには必ずSnapshotを作成する。
7. AI結果は元データと分離して保存する。
8. 多対多関係は中間エンティティで管理する。
9. 金額・税率・通貨は曖昧な形式で保存しない。
10. 日付・日時・タイムゾーンの扱いを統一する。
11. 機密情報を通常テーブルへ平文保存しない。
12. 物理削除よりArchiveまたはSoft Deleteを優先する。
13. 業務状態とデータ保存状態を分ける。
14. 過去の記録がプロフィール変更で書き換わらない構造とする。
15. UI都合だけの重複データを作らない。

---

# Summary

本サービスのデータ構造は、以下の考え方を中心とする。

```text
Organization
    │
    ▼
Contact
    │
    ├── Conversation Group
    │       └── Conversation Source
    │               └── Message
    │
    └── Project
            ├── Task
            ├── Document
            │       └── Document Snapshot
            └── Accounting Entry
```

仕事は人との会話から始まり、案件・タスク・書類・会計へ発展する。

AIはこの流れを解析し、整理・分類・提案を行う。

ただし、データを管理し、判断し、仕事を進める主体は常にユーザーである。
