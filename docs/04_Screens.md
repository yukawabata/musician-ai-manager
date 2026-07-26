# 04 Screens

## Purpose

本ドキュメントは、本サービスに存在する各画面の目的・責任・主要機能を定義する。

本ドキュメントでは、詳細なレイアウトやピクセル単位の配置は定義しない。

画面の具体的な構成は、以下のドキュメントで定義する。

- `11_Wireframes.md`
- `12_Components.md`

本ドキュメントが定義するのは、主に以下である。

- 画面が存在する目的
- ユーザーが画面で達成すること
- 表示する情報
- ユーザーが行える操作
- AIが補助できる範囲
- MVPで実装する範囲
- 将来的な拡張範囲

---

# Screen Philosophy

本サービスは、フリーランスや個人事業主のためのBusiness Relationship OSである。

一般的な業務管理ツールのように、ProjectやTaskを起点として仕事を管理するのではない。

仕事が生まれる実際の流れに沿って設計する。

```text
Conversation
↓
Project
↓
Task
↓
Document
↓
Accounting
```

すべての仕事はConversationから始まる。

そのため、本サービスではConversationを画面設計の中心に置く。

---

# Product Constitution

本サービスにおけるAIの基本原則は以下とする。

```text
AIは判断しない
AIは主張しない
AIは裏方である
```

AIはユーザーに代わって意思決定を行わない。

AIは情報を整理し、候補を提示し、入力を補助する。

最終的な判断と実行は常にユーザーが行う。

---

# Screen Design Principles

## 1. Conversation First

仕事の入口はConversationである。

HomeではConversation一覧を表示し、ConversationからProjectやTaskへつなげる。

ProjectやPeopleを利用するために、必ず特定の画面階層を経由する必要はない。

---

## 2. Context Over Navigation

画面遷移よりも、現在の文脈を維持することを優先する。

ユーザーがConversationを確認している場合、そのConversationに関連するProject、Task、Documentなどを同じ画面内で確認できるようにする。

---

## 3. Progressive Disclosure

最初からすべての情報を表示しない。

必要性の高い情報を先に表示し、詳細情報はスクロール、展開、または遷移によって表示する。

---

## 4. AI in Background

AIを独立した主役の画面として扱わない。

AIの提案は、対象となる情報の近くに小さく表示する。

ChatGPTのような大きなAI入力画面を、通常の業務フローの中心には配置しない。

---

## 5. Human in Control

AIによる以下の操作は禁止する。

- Messageの自動送信
- Projectの自動作成
- Contactの自動統合
- Taskの自動確定
- Documentの自動発行
- Accountingデータの自動確定

AIは候補を提示し、ユーザーが確認した後に反映する。

---

## 6. One Screen Workflow

可能な限り、一つの画面内で仕事を完了できるようにする。

特にConversationとProject Detailでは、関連する情報を長いスクロール画面の中にまとめる。

---

## 7. Mobile First

画面はスマートフォンでの利用を基準として設計する。

デスクトップ版では、モバイル版の構造を維持しながら表示領域を拡張する。

---

## 8. Less Input

ユーザーによる手入力を最小限にする。

既存のConversation、Contact、Project、Documentから再利用できる情報は自動入力候補として提示する。

---

## 9. Less Thinking

ユーザーにシステム上の分類や複雑な操作方法を考えさせない。

ユーザーが考えるべきことは、基本的に以下だけである。

```text
今、何をするべきか
```

---

# UI Naming

本サービスでは、内部データモデル名とユーザーへ表示する画面名を分離する。

内部データモデルはシステム設計上の一貫性を優先し、画面名はユーザーが直感的に理解できる名称を採用する。

| Data Model | UI Name |
|---|---|
| Conversation | Home / Conversation |
| Contact | People / Person Detail |
| Project | Projects / Project Detail |
| Task | Tasks |
| Document | Documents / Document Detail |
| Accounting | Accounting |
| Transaction | Transaction Detail |

仕様書およびDatabase設計ではData Model名を使用する。

画面設計、UI、ナビゲーションではUI Nameを使用する。

---

# Screen Layers

本サービスの画面は、以下のLayerに分類する。

```text
Communication
│
├ Home
└ Conversation

Relationship
│
├ People
└ Person Detail

Work
│
├ Projects
├ Project Detail
└ Tasks

Output
│
├ Documents
└ Document Detail

Finance
│
├ Accounting
└ Transaction Detail

Utility
│
├ Search
└ Settings
```

---

# Navigation

## Primary Navigation

主要ナビゲーションでは以下へアクセスできる。

- Home
- People
- Projects
- Documents
- Accounting

SearchおよびSettingsは補助ナビゲーションとして配置する。

---

## Navigation Principle

PeopleはConversationとProjectの間に必須の階層として配置しない。

Conversationから直接Projectへ進むことができる。

Peopleは、ConversationとProjectを横断するRelationship Hubとして機能する。

```text
Conversation
↘
  People
↗
Project
```

同様に、DocumentやAccountingもProject Detailから直接アクセスできる。

---

# Common Screen Structure

各画面は、原則として以下の項目で定義する。

```text
Layer
Purpose
Primary Goal
Design Philosophy
Display
User Actions
AI Support
Navigation
Empty State
Error State
Loading State
MVP Scope
MVP Simplifications
Future
Success Criteria
```

---

# Home

## Layer

Communication

---

## Purpose

Homeは本サービスの起点となる画面である。

ユーザーはHomeを開くことで、以下を自然に把握できる。

- 新しい連絡
- 返信が必要な相手
- 現在進行中の仕事
- 今日対応する必要があるConversation

Homeはダッシュボードではない。

ユーザーが仕事を始めるためのConversation一覧として設計する。

---

## Primary Goal

ユーザーが5秒以内に、以下を判断できること。

```text
今、誰と話すべきか
```

---

## Design Philosophy

Homeの主役はConversationである。

AIによる要約、売上グラフ、Projectの統計情報などを画面の中心には配置しない。

仕事に関する情報は、Conversation一覧の補足情報として控えめに表示する。

Homeは、LINEなどのメッセージアプリに近い直感的な一覧画面とする。

---

## Display

### Action Filters

Conversation一覧の上部に、現在必要な行動を小さく表示する。

例

```text
返信する 2
今日締切 1
未請求 3
```

各項目を選択すると、条件に該当するConversationだけを一覧表示する。

Action Filtersは独立したダッシュボードではなく、Conversation一覧のフィルターとして機能する。

---

### Conversation List

Conversation Groupを一覧表示する。

表示内容

- Conversation Group名
- アイコン
- 最後のMessage
- 最終更新日時
- 未読件数
- Conversation Source
- ラベル
- 要返信状態
- 関連Projectの簡易状態

表示例

```text
○ ○○高校
来週までに修正版をお願いします
🟠 要返信
昨日
```

Conversationは、原則として最終更新日時順に並べる。

ただし、ユーザーによるピン留めやフィルターが有効な場合は、その条件を優先する。

---

### Business Indicators

仕事に関する情報は、Conversation一覧に小さく表示する。

表示候補

- 要返信
- 今日締切
- 進行中
- 見積中
- 未請求
- 未入金
- Project候補

CRMのような大量の項目は表示しない。

Conversationの可読性を優先する。

---

### Floating Action Button

画面右下にFloating Action Buttonを表示する。

作成可能な対象

- Project
- Task
- Document
- Contact

Conversationは外部サービスとの連携によって生成されるため、通常の新規作成対象には含めない。

---

## User Actions

ユーザーは以下を行える。

- Conversationを開く
- 未読・既読を変更する
- Conversationをピン留めする
- 検索する
- Action Filterを選択する
- ラベルで絞り込む
- 並び替える
- Projectを作成する
- Taskを作成する
- Documentを作成する
- Contactを作成する

---

## AI Support

AIはConversation単位で以下を補助する。

- 要返信候補
- 優先対応候補
- Project候補
- 納期候補
- 請求漏れ候補
- Conversationの簡易要約

AIによる優先順位は確定情報として扱わない。

ユーザーが並び順を変更したり、AI候補を無視したりできること。

---

## Navigation

Homeから以下へ遷移できる。

- Conversation
- People
- Projects
- Documents
- Accounting
- Search
- Settings

---

## Empty State

Conversationが存在しない場合は、連携サービスの設定を案内する。

表示例

```text
まだConversationがありません

Gmailやその他のサービスを連携すると、
仕事の連絡がここに表示されます。
```

---

## Error State

Conversationの取得に失敗した場合は以下を表示する。

- 取得失敗したConversation Source
- 最終同期日時
- 再試行
- 接続設定への導線

一部のサービスで取得に失敗しても、取得済みのConversationは表示する。

---

## Loading State

Conversation一覧のスケルトンを表示する。

既存データがある場合は、キャッシュされた一覧を表示しながら同期する。

---

## MVP Scope

MVPでは以下を実装する。

- Conversation一覧
- 最後のMessage表示
- 最終更新日時
- 未読件数
- Conversation Source表示
- ラベル表示
- 要返信表示
- Action Filters
- 検索
- Floating Action Button

---

## MVP Simplifications

MVPでは以下を簡略化できる。

- Action Filtersは固定項目とする
- 並び順は更新日時順を基本とする
- AI優先順位は表示のみとする
- ピン留めは任意実装とする
- ジェスチャー操作は実装しない

---

## Future

正式版以降で以下を検討する。

- カスタムAction Filters
- Conversationのピン留め
- お気に入り
- スマートフィルター
- AIによる優先順位の個人最適化
- スワイプ操作
- カスタム並び順
- チーム担当者表示
- 複数アカウント切り替え

---

## Success Criteria

Homeを開いた瞬間、ユーザーが以下を迷わないこと。

```text
今、誰と話すべきか
```

Conversation一覧から直接仕事を開始できることを目標とする。

---

# Conversation

## Layer

Communication

---

## Purpose

Conversationは、外部サービスから取得したMessageを確認し、返信や案件化などの仕事へつなげる画面である。

Conversationは単なるチャット画面ではない。

会話を起点として、以下へ自然につながる仕事の入口として設計する。

- Contact
- Project
- Task
- Document
- Accounting

---

## Primary Goal

ユーザーが会話の内容と仕事の状況を同時に把握し、画面を大きく移動せず次の行動へ進めること。

---

## Design Philosophy

Conversationの主役はMessageである。

Contact情報やProject情報を先に見せるのではなく、会話を最優先で表示する。

仕事に関する情報は、会話の後に続く一つの長いスクロール画面として表示する。

基本構造は以下とする。

```text
Conversation Header
↓
Message Timeline
↓
Message Input
↓
Related Projects
↓
Related Tasks
↓
Related Documents
↓
Related Accounting
↓
Related People
```

---

## Conversation Header

画面上部にConversation Groupの基本情報を表示する。

表示内容

- Conversation Group名
- アイコン
- 参加Contact
- Conversation Source
- ラベル
- 未読状態
- メニュー

Conversation Sourceが複数ある場合は、接続されているサービスを表示する。

例

- Gmail
- LINE
- Instagram
- X
- Facebook

---

## Message Timeline

Conversation Groupに含まれるMessageを時系列で表示する。

表示内容

- 送信者
- 本文
- 送受信日時
- Conversation Source
- 添付ファイル
- 既読状態
- 送信状態

複数のConversation Sourceを一つのConversation Groupに統合している場合も、同一のTimeline内に表示する。

各MessageがどのConversation Sourceから取得されたものかを常に判別できること。

---

## Message Input

画面内に返信入力欄を表示する。

入力欄では以下を行える。

- テキスト入力
- 添付ファイル追加
- 送信先Conversation Sourceの選択
- 下書き保存
- 返信候補の挿入
- 送信

Message Timelineの閲覧を妨げない範囲で、入力欄を画面下部に固定できる。

AIが自動でMessageを送信することはない。

---

## Reply Source

Conversation Groupに複数のConversation Sourceが含まれる場合、ユーザーは送信先を選択できる。

例

```text
送信先

LINE
Gmail
Instagram
```

直前のMessageと同じConversation Sourceを初期候補として表示する。

最終決定はユーザーが行う。

---

## Related Work Summary

Message Inputの後に、関連する仕事情報の要約を表示する。

表示例

```text
関連する仕事

進行中Project 2
未完了Task 4
Document 1
未入金 1
```

各項目は展開可能とする。

---

## Related Projects

Conversation Groupに関連するProjectを表示する。

表示内容

- Project名
- ステータス
- 納期
- 金額
- 未完了Task数
- 最終更新日時

Primary Projectが設定されている場合は、最初に表示する。

---

## Related Tasks

Conversationおよび関連Projectに紐づくTaskを表示する。

表示内容

- Task名
- 期限
- ステータス
- 関連Project

Conversation画面からTaskを完了できるようにする。

---

## Related Documents

Conversationまたは関連Projectに紐づくDocumentを表示する。

表示対象

- 見積書
- 請求書
- 契約書
- 納品書
- その他の書類

表示内容

- Document種別
- Document名
- ステータス
- 発行日
- 金額

---

## Related Accounting

Conversationに関連する入出金状況を要約表示する。

表示内容

- 見積金額
- 請求金額
- 入金状況
- 支払期限
- 未回収金額

Accounting情報は必要な場合のみ展開する。

---

## Related People

Conversation Groupに参加しているContactを表示する。

表示内容

- 名前
- 所属
- 役職
- Contact Channel
- メモ

People情報は、Messageよりも優先して表示しない。

---

## User Actions

ユーザーは以下を行える。

- Messageを読む
- Messageを送信する
- 添付ファイルを確認する
- 送信先Conversation Sourceを選択する
- Conversationを既読にする
- ラベルを追加・削除する
- Conversation Group名を変更する
- Person Detailを開く
- Contactを作成する
- Projectを作成する
- 既存Projectへ紐づける
- Taskを作成する
- Taskを完了する
- Documentを作成する
- Conversation Sourceを統合する
- Conversation Groupを分割する

---

## Project Creation

ConversationからProjectを作成できる。

Project作成時は、Conversationの内容から以下を候補として表示する。

- Project名
- Contact
- 仕事内容
- 納期
- 金額
- Task
- 関連Message

ユーザーが内容を確認し、承認した時点でProjectを作成する。

AIが自動でProjectを作成することはない。

---

## Contact Creation

未登録の相手とのConversationでは、Contact作成候補を表示できる。

候補情報

- 名前
- 所属
- 役職
- メールアドレス
- 電話番号
- Conversation Source上の表示名

ユーザーが確認した後にContactを作成する。

---

## Conversation Group Management

ユーザーは複数のConversation Sourceを一つのConversation Groupへ統合できる。

例

```text
山田さん

├ LINE
├ Gmail
└ Instagram
```

誤って統合したConversation Sourceは分離できる。

AIは統合候補を提案できるが、自動統合は行わない。

---

## Labels

Conversation Groupには複数のLabelを付与できる。

例

- 要返信
- 重要
- 学校
- 継続案件
- 見積中

ユーザーが作成したLabelと、AIによる分類情報は分けて保持する。

---

## AI Support

AIはConversation内で以下を補助できる。

- 会話要約
- 要返信判定
- Project候補
- Contact候補
- 納期候補
- 金額候補
- Task候補
- Document候補
- Conversation統合候補
- 返信文候補

AIの提案は、対象となる情報の近くに小さく表示する。

---

## Reply Suggestions

AIは返信文の候補を作成できる。

返信候補は下書きとしてMessage Inputに反映する。

ユーザーは以下を行える。

- 内容を編集する
- 採用しない
- 再生成する
- 送信する

AIがユーザーの確認なしに送信することはない。

---

## Navigation

Conversationから以下へ遷移できる。

- Home
- Person Detail
- Project Detail
- Documents
- Accounting
- Search
- Settings

関連情報を開いた後も、元のConversationへ戻りやすい構造とする。

---

## Empty State

Messageが存在しない場合は以下を表示する。

```text
まだMessageはありません
```

Conversation Sourceの連携待ちの場合は、その状態を明示する。

---

## Error State

Message取得に失敗した場合は以下を表示する。

- 取得失敗
- 最終同期日時
- 再試行
- Conversation Sourceの接続状態

一部のConversation Sourceだけ取得できない場合も、取得済みのMessageは表示する。

---

## Loading State

初回表示時はMessage Timelineのスケルトンを表示する。

過去Messageを追加取得する場合は、現在の表示位置を維持する。

---

## MVP Scope

MVPでは以下を実装する。

- Conversation Header
- Message Timeline
- Message Input
- Conversation Source表示
- Conversation Group単位の表示
- 未読管理
- Label管理
- Related Work Summary
- Contact表示
- Project表示
- ConversationからのProject作成
- ConversationからのTask作成
- AIによる要返信候補
- AIによるProject候補
- AIによるContact候補
- 手動によるConversation Source統合

---

## MVP Simplifications

MVPでは以下を簡略化できる。

- Conversation Sourceの切り替えは選択式
- Related Workは要約表示を中心とする
- Conversation Group分割は設定メニューから行う
- 高度なMessage検索はSearch画面で行う
- AI返信候補は一案のみでもよい
- リアルタイム更新は対応可能なサービスから段階的に実装する

---

## Future

正式版以降で以下を検討する。

- 音声Message
- Messageへのリアクション
- Messageの予約送信
- 定型文
- 返信テンプレート
- 複数人Conversation
- メンション
- 内部メモ
- Conversationの共有
- Organization内担当者設定
- 自動翻訳
- 高度なAI要約
- AIによる会話履歴比較
- 送信前チェック
- センシティブ情報検出
- 添付ファイル内容の解析
- MessageからのDocument下書き作成
- Project進捗に応じた返信候補

---

## Success Criteria

Conversationを開いたユーザーが、以下を一つの画面で把握できること。

- 誰からの連絡か
- 何を話しているか
- 返信が必要か
- どのProjectに関係するか
- 次に何をすべきか

Conversationの確認から、返信、案件化、Task作成までが自然につながることを目標とする。

---

# People

## Layer

Relationship

---

## Purpose

Peopleは、仕事で関わる相手を一覧表示する画面である。

人物、法人、団体、学校、自治体など、すべてのContactを一つの一覧として表示する。

Peopleは住所録ではない。

現在および過去の仕事相手へ素早くアクセスするためのRelationship Browserとして設計する。

---

## Primary Goal

ユーザーが目的の相手を数秒で見つけること。

---

## Design Philosophy

Peopleは顧客を管理するためのCRM画面ではない。

ユーザーが仕事上の関係を振り返り、ConversationやProjectへアクセスするための横断的な入口とする。

Contactを細かく分類することよりも、探しやすさと仕事の文脈を優先する。

---

## Contact Types

Database上では、以下をすべてContactとして扱う。

- 個人
- 法人
- 団体
- 学校
- 自治体
- その他の組織

UIでは、必要に応じて種別を表示できる。

種別ごとに完全に別の画面構造は作らない。

---

## Display

People一覧では以下を表示する。

- 名前
- アイコン
- 所属
- Contact種別
- 最新Conversationの一部
- 進行中Project数
- 要返信状態
- ラベル
- 最終更新日時

表示例

```text
○ 山田太郎
株式会社○○
進行中Project 2
昨日
```

---

## Sorting

ユーザーは以下の条件で並び替えられる。

- 最近更新
- 名前
- 進行中Project数
- Contact作成日
- 最終Conversation日時

初期状態では最近更新されたContactから表示する。

---

## Filtering

ユーザーは以下で絞り込める。

- ラベル
- Contact種別
- 所属
- Project進行中
- 要返信
- お気に入り
- 最近やり取りした相手

---

## Search

People画面では以下の情報から検索できる。

- 名前
- 読み方
- 所属
- 部署
- 役職
- メールアドレス
- 電話番号
- Conversation Source上の表示名
- メモ

People内の検索結果からGlobal Searchへ切り替えられる。

---

## User Actions

ユーザーは以下を行える。

- Person Detailを開く
- Contactを作成する
- Contactを編集する
- Contactを削除する
- ラベルを付与する
- お気に入りに追加する
- 検索する
- 並び替える
- 絞り込む
- Conversationを開く
- Projectを作成する

---

## AI Support

AIは以下を提案できる。

- 新規Contact候補
- 重複Contact候補
- 不足情報
- 所属候補
- Contact種別候補
- ConversationとContactの紐付け候補

AIがContactを自動作成または自動統合することはない。

---

## Navigation

Peopleから以下へ遷移できる。

- Person Detail
- Conversation
- Project Detail
- Search

---

## Empty State

Contactが存在しない場合は以下を表示する。

```text
まだPeopleが登録されていません

Conversationから仕事相手を登録するか、
新しいContactを作成してください。
```

---

## Error State

Peopleの取得に失敗した場合は以下を提供する。

- 再読み込み
- キャッシュ表示
- Contact新規作成
- 連携状態の確認

---

## Loading State

People一覧のスケルトンを表示する。

アイコン画像の読み込みを待たず、名前と基本情報を先に表示する。

---

## MVP Scope

MVPでは以下を実装する。

- Contact一覧
- 名前表示
- 所属表示
- 最新Conversation表示
- 進行中Project数
- 検索
- 並び替え
- ラベル
- Contact作成
- Person Detailへの遷移

---

## MVP Simplifications

MVPでは以下を簡略化できる。

- Contact種別は任意入力とする
- アイコン画像は任意とする
- 並び替え条件は最近更新と名前のみでもよい
- お気に入りはFutureへ移動可能
- 高度な所属検索は実装しない

---

## Future

正式版以降で以下を検討する。

- Organization単位の表示
- 組織ツリー
- Relationship Map
- Contact Analytics
- AI人物分類
- 名刺OCR
- QRコードによるContact交換
- 地図表示
- チーム内担当者表示
- 関係性の可視化

---

## Success Criteria

ユーザーが仕事相手を迷わず探し、Person Detail、Conversation、Projectへ素早く移動できること。

---

# Person Detail

## Layer

Relationship

---

## Purpose

Person Detailは、特定のContactに関する情報と仕事上の関係を確認する画面である。

ConversationやProjectから参照される人物または組織の情報を一元的に表示する。

Person Detailはプロフィール画面や住所録ではない。

以下を把握するためのRelationship Hubとして設計する。

```text
この相手と、
いつ、どのような話をして、
どのような仕事をしてきたか
```

---

## Primary Goal

ユーザーが以下を数秒で把握できること。

```text
この相手と今までどのような仕事をしてきたか
```

---

## Design Philosophy

Person Detailは、Contactの基本情報よりも仕事の文脈を重視する。

Contact自体が仕事を所有するのではなく、以下の情報を横断する入口として機能する。

- Conversation
- Project
- Task
- Document
- Accounting

Person Detailを経由しなければProjectへ移動できない構造にはしない。

---

## Screen Structure

Person Detailは、一つの長いスクロール画面を基本とする。

```text
Person Header
↓
Current Status
↓
Recent Conversations
↓
Projects
↓
Documents
↓
Accounting Summary
↓
Relationship Timeline
↓
Basic Information
↓
Notes
```

---

## Person Header

画面上部にContactの識別情報を表示する。

表示内容

- 名前
- アイコン
- 所属
- 部署
- 役職
- Contact種別
- ラベル
- お気に入り状態

主な操作

- Conversationを開く
- Projectを作成する
- Contactを編集する
- メニューを開く

---

## Current Status

Contactとの現在の仕事状況を要約表示する。

表示候補

- 要返信Conversation
- 進行中Project
- 今日締切のTask
- 未請求
- 未入金
- 最新のやり取り日時

表示例

```text
現在の状況

要返信 1
進行中Project 2
未入金 1
```

---

## Contact Channels

Contactに紐づく連絡先を表示する。

例

- Gmail
- LINE
- Instagram
- Facebook
- X
- 電話番号
- その他

一つのContactに複数のContact Channelを登録できる。

---

## Recent Conversations

最近のConversationを表示する。

表示内容

- Conversation Group名
- 最後のMessage
- 最終更新日時
- Conversation Source
- 要返信状態
- 関連Project

Conversationを選択するとConversation画面へ遷移する。

---

## Projects

Contactに関連するProjectを表示する。

表示内容

- Project名
- ステータス
- 納期
- 金額
- 未完了Task数
- 最終更新日時

進行中Projectを優先して表示し、その後に完了済みProjectを表示する。

---

## Documents

Contactに関連するDocumentを表示する。

表示対象

- 見積書
- 請求書
- 契約書
- 納品書
- その他の書類

表示内容

- Document名
- 種別
- ステータス
- 発行日
- 金額
- 関連Project

---

## Accounting Summary

Contactとの取引状況を要約表示する。

表示候補

- 累計売上
- 見積中金額
- 未請求金額
- 未入金金額
- 最終請求日
- 最終入金日

詳細はAccounting画面で確認する。

---

## Relationship Timeline

Contactに関連する履歴を時系列で表示する。

表示対象

- Contact作成
- Conversation
- Project開始
- Task完了
- 見積書作成
- 契約
- 納品
- 請求
- 入金
- メモ追加

TimelineにはすべてのMessageを表示せず、重要なイベントのみを表示する。

---

## Basic Information

Contactの基本情報を表示する。

表示候補

- 名前
- 読み方
- Contact種別
- 所属
- 部署
- 役職
- メールアドレス
- 電話番号
- Webサイト
- 住所
- SNS
- 備考

未入力の項目を常に表示する必要はない。

---

## Notes

ユーザーがContactに関する自由記述メモを保存できる。

メモ例

- 連絡可能な時間帯
- 呼び方
- 支払い条件
- 好み
- 過去の注意事項
- 次回話す内容

AIがConversationからメモ候補を提示できる。

ユーザーの承認なしに保存しない。

---

## User Actions

ユーザーは以下を行える。

- Contactを編集する
- Contactを削除する
- Conversationを開く
- 新しいConversation Sourceを紐づける
- Projectを作成する
- Taskを作成する
- Documentを作成する
- ラベルを追加・削除する
- メモを追加・編集する
- Contactを統合する
- Contact Channelを追加・削除する

---

## Contact Merge

重複するContactを一つに統合できる。

統合前に以下を表示する。

- 統合対象の基本情報
- Conversation
- Project
- Document
- Accounting
- Contact Channel
- 競合している項目

ユーザーが残す情報を確認した後に統合する。

AIによる自動統合は行わない。

---

## AI Support

AIは以下を補助できる。

- Contact候補
- 重複Contact候補
- 不足情報候補
- 所属候補
- 役職候補
- Contact Channel候補
- Conversationとの紐付け候補
- 人物・組織の簡易要約
- メモ候補
- 次回確認事項の候補

人物評価や信用度などの断定的な判断は行わない。

---

## Navigation

Person Detailから以下へ遷移できる。

- People
- Conversation
- Project Detail
- Document Detail
- Accounting
- Search

---

## Empty State

仕事履歴がない場合は以下を表示する。

```text
このPeopleとの仕事はまだありません
```

Conversationのみ存在する場合は、Conversationを優先して表示する。

---

## Error State

一部の関連情報を取得できない場合でも、取得済みのContact情報は表示する。

以下を提供する。

- 再読み込み
- Contact編集
- キャッシュ表示
- 接続状態の確認

---

## Loading State

Person Headerを先に表示し、その後に関連データを読み込む。

各Sectionは独立してLoading Stateを持つことができる。

---

## MVP Scope

MVPでは以下を実装する。

- Person Header
- Basic Information
- Contact Channel
- Recent Conversations
- Projects
- Documents
- Accounting Summary
- メモ
- Contact編集
- AIによる重複候補

---

## MVP Simplifications

MVPでは以下を簡略化できる。

- Relationship TimelineはConversationとProjectのみ
- Accounting Summaryは未請求・未入金のみ
- Person Summaryは実装しない
- Contact Mergeは基本情報のみの比較でもよい
- SNSの自動取得は実装しない

---

## Future

正式版以降で以下を検討する。

- 名刺OCR
- QRコード交換
- Organization Tree
- 担当者管理
- 商談履歴
- AI人物要約
- 誕生日
- 地図表示
- Relationship Analytics
- 関係性の変化検出
- チーム内共有メモ
- Contactごとの権限管理

---

## Success Criteria

Person Detailを開いたユーザーが、以下を数秒で理解できること。

- 誰なのか
- 最近何を話したか
- どの仕事が進んでいるか
- 未対応事項があるか
- 過去にどのような仕事をしたか
- 次にどのConversationまたはProjectを見るべきか

---

# Projects

## Layer

Work

---

## Purpose

Projectsは、現在および過去の仕事を一覧表示する画面である。

ユーザーが進行中の仕事を見つけ、現在の状態を把握し、Project Detailへアクセスするための入口として設計する。

Projectsは、仕事を細かく管理するための複雑な管理表ではない。

以下を素早く把握するためのWork Browserである。

```text
今、どの仕事が動いているか
```

---

## Primary Goal

ユーザーが進行中の仕事と、次に対応が必要なProjectを数秒で見つけること。

---

## Design Philosophy

Projectsは、すべてのProjectを同じ重さで表示しない。

ユーザーの行動が必要なProjectを優先する。

ただし、AIによる優先順位をユーザーへ強制しない。

Projectの一覧は、表計算ソフトのような大量の列を持つ形式ではなく、モバイルで読みやすいリスト形式を基本とする。

---

## Project States

Projectは以下の基本ステータスを持つ。

- Draft
- Proposed
- Active
- Waiting
- Completed
- Cancelled
- Archived

UIでは日本語表示を利用できる。

| Internal Status | UI |
|---|---|
| Draft | 下書き |
| Proposed | 提案・見積中 |
| Active | 進行中 |
| Waiting | 相手待ち |
| Completed | 完了 |
| Cancelled | 中止 |
| Archived | アーカイブ |

ステータスは業種ごとにカスタマイズ可能にすることを将来的に検討する。

---

## Display

Project一覧では以下を表示する。

- Project名
- Primary Contact
- ステータス
- 納期
- 次のTask
- 未完了Task数
- 金額
- 要返信状態
- 未請求・未入金状態
- 最終更新日時

表示例

```text
○ ○○高校 編曲依頼
山田先生

次：初稿を提出
締切：8月10日
🟠 要返信
```

すべての項目を常時表示せず、重要度の高い情報に限定する。

---

## Priority Indicators

Projectに対応が必要な場合、小さなIndicatorを表示する。

表示候補

- 今日締切
- 期限超過
- 要返信
- 相手待ち
- 見積未送付
- 未請求
- 未入金
- 納品待ち
- Project候補から未確定

Indicatorは警告を過剰に表示しない。

一つのProjectに多数の問題がある場合は、代表的なものだけを一覧に表示する。

---

## Views

Projectsでは以下のViewを提供する。

### Active

現在進行中のProjectを表示する。

### Waiting

相手からの返信、確認、入金などを待っているProjectを表示する。

### Completed

完了済みProjectを表示する。

### All

すべてのProjectを表示する。

MVPでは、Viewをタブまたはフィルターとして実装できる。

---

## Sorting

ユーザーは以下の条件で並び替えられる。

- 対応優先
- 納期
- 最終更新
- Project名
- 金額
- 作成日

初期状態では、対応が必要なProjectを優先し、その後に納期または最終更新日時を考慮する。

AIによる並び順の場合は、その旨を控えめに表示する。

---

## Filtering

ユーザーは以下で絞り込める。

- ステータス
- Contact
- ラベル
- 納期
- 要返信
- 未請求
- 未入金
- Project種別
- 金額
- 完了・未完了

---

## Search

Projects内では以下から検索できる。

- Project名
- Contact名
- Project概要
- Conversation内容
- Task名
- Document名
- メモ
- ラベル

Global Searchへ切り替えることもできる。

---

## Project Creation

Projects画面から新しいProjectを作成できる。

最低限必要な入力項目はProject名のみとする。

その他の情報は、作成後に追加できる。

入力候補

- Project名
- Contact
- Conversation
- 概要
- 納期
- 金額
- ステータス
- 最初のTask

Project作成時にすべての項目を埋めることを要求しない。

---

## User Actions

ユーザーは以下を行える。

- Project Detailを開く
- Projectを作成する
- Projectを編集する
- ステータスを変更する
- Projectを複製する
- Projectをアーカイブする
- 検索する
- 絞り込む
- 並び替える
- ラベルを付与する
- Projectを完了する

---

## AI Support

AIは以下を補助できる。

- ConversationからのProject候補
- Project名候補
- Contact候補
- 納期候補
- 金額候補
- Project概要候補
- 次のTask候補
- 要対応Project候補
- 重複Project候補
- Projectステータス変更候補

AIがProjectを自動作成または自動完了することはない。

---

## Navigation

Projectsから以下へ遷移できる。

- Project Detail
- Conversation
- Person Detail
- Documents
- Accounting
- Search

---

## Empty State

Projectが存在しない場合は以下を表示する。

```text
まだProjectがありません

Conversationから仕事をProjectにするか、
新しいProjectを作成してください。
```

Active ViewにProjectが存在しない場合は、完了済みProjectへの導線を表示できる。

---

## Error State

Project一覧の取得に失敗した場合は以下を提供する。

- 再読み込み
- キャッシュ表示
- Project新規作成
- フィルター解除

---

## Loading State

Project Listのスケルトンを表示する。

ステータス変更などの軽微な操作では、画面全体を再読み込みしない。

---

## MVP Scope

MVPでは以下を実装する。

- Project一覧
- Project名
- Primary Contact
- ステータス
- 納期
- 次のTask
- 未完了Task数
- 要返信表示
- 未請求・未入金表示
- View切り替え
- 検索
- 絞り込み
- Project作成
- Project Detailへの遷移

---

## MVP Simplifications

MVPでは以下を簡略化できる。

- ViewはActive、Waiting、Completedのみ
- 並び替えは最終更新と納期のみ
- 金額は任意表示
- Project種別は実装しない
- AI並び替えはFutureへ移動可能
- Project複製は実装しない

---

## Future

正式版以降で以下を検討する。

- カスタムProject Status
- Kanban View
- Calendar View
- Timeline View
- Project Template
- Project複製
- 定期Project
- チーム担当者
- Project収益分析
- 稼働時間分析
- AIによるリスク検出
- 外部カレンダー連携
- Project間の依存関係

---

## Success Criteria

Projectsを開いたユーザーが以下を迷わず把握できること。

- 現在進行中の仕事
- 次に対応する仕事
- 納期が近い仕事
- 相手からの返信を待っている仕事
- 請求または入金対応が必要な仕事

---

# Project Detail

## Layer

Work

---

## Purpose

Project Detailは、一つの仕事に必要な情報と行動をまとめるWorkspaceである。

Projectに関連するConversation、Task、Document、Accounting、File、Note、Timelineを一つの画面に集約する。

Project Detailは、Projectの基本情報を閲覧するだけの画面ではない。

ユーザーが実際に仕事を進める場所として設計する。

---

## Primary Goal

ユーザーがProject Detailを開くだけで、以下を把握し、次の行動を実行できること。

- 何の仕事か
- 誰との仕事か
- どこまで進んでいるか
- 最後に何を話したか
- 次に何をするか
- 納期はいつか
- 書類や請求の状態はどうか

---

## Design Philosophy

Project Detailは、Project情報を起点とする管理画面ではない。

Conversationの文脈を保ったまま仕事を実行するWorkspaceとする。

基本構造は以下とする。

```text
Project Header
↓
Current Status
↓
Recent Conversation
↓
Next Action
↓
Tasks
↓
Documents
↓
Accounting
↓
Files
↓
Notes
↓
Timeline
↓
Project Information
```

各Sectionは一つの長いスクロール画面として表示する。

タブを主要構造として使用しない。

ただし、長い画面内を移動するためのSection Navigationは使用できる。

---

## Project Header

画面上部にProjectの識別情報を表示する。

表示内容

- Project名
- Primary Contact
- ステータス
- 納期
- ラベル
- 最終更新日時

主な操作

- Messageを送る
- Taskを追加する
- Documentを作成する
- Projectを編集する
- ステータスを変更する
- メニューを開く

---

## Current Status

Projectの現在状況を簡潔に表示する。

表示候補

- ステータス
- 次のTask
- 納期までの日数
- 要返信
- 相手待ち
- 未請求
- 未入金
- 最終Conversation日時

表示例

```text
現在の状況

次：初稿を提出
締切まで 4日
要返信なし
見積送付済み
```

このSectionは分析結果を大量に表示する場所ではない。

ユーザーが次の行動を理解するために必要な情報だけを表示する。

---

## Recent Conversation

Projectに関連するConversationを表示する。

表示内容

- Conversation Group名
- Primary Contact
- 最新Message
- 最終更新日時
- Conversation Source
- 要返信状態

最新のConversationをProject Detail内に直接表示する。

表示例

```text
山田先生

「ありがとうございます。
修正版を来週までにお願いします。」

昨日 18:42・LINE
```

ユーザーは以下を行える。

- Conversationを開く
- Project Detail内から返信する
- 関連Messageを追加する
- Conversationとの紐付けを解除する

Conversationが複数ある場合は、主要なConversationを最初に表示する。

---

## Next Action

Projectにおける次の行動を一つ、明確に表示する。

Next Actionは以下から決定される。

- ユーザーが指定したTask
- 期限が最も近い未完了Task
- 要返信Conversation
- 未送付Document
- 未請求
- 入金確認

AIはNext Action候補を提案できる。

ユーザーは候補を変更または無視できる。

表示例

```text
次にやること

初稿PDFを山田先生へ送る
8月10日まで
```

---

## Tasks

Projectに関連するTaskを表示する。

表示内容

- Task名
- ステータス
- 期限
- 優先度
- 担当者
- 関連Conversation
- 親Task

Taskは以下の状態を持つ。

- To Do
- In Progress
- Waiting
- Completed
- Cancelled

Project Detail内で以下を行える。

- Task作成
- Task編集
- Task完了
- Task並び替え
- 期限変更
- Waitingへの変更
- Next Actionへの指定

完了済みTaskは折りたたんで表示できる。

---

## Task Creation

Task作成時の必須項目はTask名のみとする。

入力候補

- Task名
- 期限
- ステータス
- 関連Message
- 担当者
- メモ

Conversationから抽出されたTask候補を表示できる。

ユーザーが確認した後にTaskを作成する。

---

## Documents

Projectに関連するDocumentを表示する。

表示対象

- 見積書
- 請求書
- 契約書
- 納品書
- 発注書
- その他の書類

表示内容

- Document名
- Document種別
- ステータス
- 発行日
- 金額
- 送付状態
- 関連Contact

Project Detail内から新しいDocumentを作成できる。

ProjectおよびContactの情報をDocumentへ自動入力候補として引き継ぐ。

---

## Document States

Documentは種別に応じて異なる状態を持つが、共通状態として以下を使用できる。

- Draft
- Ready
- Sent
- Accepted
- Paid
- Cancelled

UIではDocument種別に応じて適切な名称へ変更できる。

---

## Accounting

Projectに関する金銭情報を表示する。

表示候補

- 見積金額
- 受注金額
- 請求金額
- 入金済み金額
- 未入金金額
- 経費
- 利益見込
- 支払期限
- 入金日

表示例

```text
Accounting

受注金額　100,000円
請求済み　100,000円
入金済み　　　 0円
未入金　　100,000円
```

Project Detailでは概要を表示し、詳細はAccounting画面で扱う。

---

## Invoice Flow

Project Detailから請求書を作成できる。

請求書作成時は以下を候補として引き継ぐ。

- Contact
- Project名
- 受注金額
- 支払期限
- 振込先
- 品目
- 備考

AIが請求内容を自動確定または自動送信することはない。

---

## Files

Projectに関連するFileを表示する。

表示対象

- 納品データ
- 参考資料
- 添付ファイル
- 画像
- 音声
- 動画
- PDF
- 楽譜
- その他

表示内容

- File名
- 種別
- 更新日時
- サイズ
- 追加元
- 関連Message
- バージョン

FileはConversationの添付ファイルからProjectへ紐づけられる。

---

## Notes

Projectに関する自由記述メモを保存できる。

メモ例

- 作業方針
- 打ち合わせ内容
- 注意事項
- アイデア
- 次回確認事項
- 内部向け情報

Notesは相手へ送信されない内部情報として扱う。

AIはConversationやDocumentからNote候補を提示できる。

ユーザーが承認した後に保存する。

---

## Timeline

Projectに関連する主要イベントを時系列で表示する。

表示対象

- Project作成
- Conversation
- ステータス変更
- Task作成・完了
- File追加
- 見積作成・送付
- 契約
- 納品
- 請求
- 入金
- Note追加
- Project完了

すべての操作ログを表示するのではなく、仕事の流れを理解するために重要なイベントを表示する。

---

## Project Information

Projectの基本情報を表示する。

表示候補

- Project名
- 概要
- Primary Contact
- 関連Contact
- 関連Conversation
- ステータス
- 開始日
- 納期
- 完了日
- 金額
- Project種別
- ラベル
- メモ

基本情報は画面上部ですべてを表示せず、下部または編集画面で確認できるようにする。

---

## Related People

Projectに関係するContactを複数登録できる。

例

- 依頼者
- 担当者
- 請求先
- 出演者
- 外注先
- 協力者

ContactごとにProject上の役割を設定できる。

MVPではPrimary Contactとその他Contactの区別のみでもよい。

---

## Project Completion

ユーザーはProjectをCompletedに変更できる。

完了時に以下の確認候補を表示する。

- 未完了Task
- 未送付Document
- 未請求
- 未入金
- 未整理File
- 次回Project候補

警告が存在しても、ユーザーはProjectを完了できる。

AIやシステムが完了を禁止しない。

---

## Project Reopening

完了済みProjectを再度Activeへ戻せる。

再開理由の入力は任意とする。

---

## User Actions

ユーザーはProject Detailで以下を行える。

- Projectを編集する
- ステータスを変更する
- Conversationを確認する
- Messageを送信する
- Conversationを紐づける
- Taskを作成する
- Taskを完了する
- Next Actionを設定する
- Documentを作成する
- Fileを追加する
- Noteを追加する
- Invoiceを作成する
- 入金を記録する
- Contactを追加する
- Projectを完了する
- Projectを再開する
- Projectをアーカイブする

---

## AI Support

AIはProject Detail内で以下を補助できる。

- Conversation要約
- Project概要候補
- 次のTask候補
- Next Action候補
- 納期候補
- 金額候補
- Contact候補
- Document候補
- 請求漏れ候補
- 入金確認候補
- Note候補
- Project完了前の確認候補
- Project進捗の簡易要約

AIは以下を行わない。

- Projectステータスの自動変更
- Taskの自動完了
- Messageの自動送信
- Documentの自動発行
- Invoiceの自動送付
- 入金の自動確定

---

## AI Summary

Projectの状況を短く要約できる。

表示例

```text
このProjectは初稿提出後、相手の確認待ちです。
次の締切は8月10日です。
請求書はまだ作成されていません。
```

AI Summaryは必要な場合のみ表示する。

画面の最上部を常に占有しない。

---

## Section Navigation

長いスクロール画面内を移動するため、簡易Section Navigationを提供できる。

例

```text
会話
Task
書類
会計
File
履歴
```

Section Navigationはタブによる画面分割ではない。

選択したSectionまでスクロールするための補助機能とする。

---

## Navigation

Project Detailから以下へ遷移できる。

- Projects
- Conversation
- Person Detail
- Document Detail
- Accounting
- Transaction Detail
- Search

遷移後もProjectへ戻りやすい導線を維持する。

---

## Empty State

Projectに関連データがないSectionでは、次の行動を提示する。

例

```text
Taskはまだありません
最初のTaskを追加
```

```text
Documentはまだありません
見積書を作成
```

```text
Conversationが紐づいていません
Conversationを選択
```

空のSection自体を完全に非表示にせず、必要な機能の存在を理解できるようにする。

---

## Error State

一部のSectionの読み込みに失敗しても、Project全体を利用できること。

各Sectionで以下を提供する。

- 再読み込み
- キャッシュ表示
- 手動入力
- 関連データへの直接遷移

---

## Loading State

Project HeaderとCurrent Statusを優先して表示する。

その他のSectionは独立して読み込む。

一つのSectionの読み込みによって、画面全体をブロックしない。

---

## MVP Scope

MVPでは以下を実装する。

- Project Header
- Current Status
- Recent Conversation
- Next Action
- Task一覧
- Task作成・完了
- Document一覧
- Document作成への導線
- Accounting Summary
- File一覧
- Notes
- Timeline
- Project Information
- Contact表示
- Project編集
- Project完了
- AIによるTask候補
- AIによるNext Action候補
- AIによるProject概要候補

---

## MVP Simplifications

MVPでは以下を簡略化できる。

- Section Navigationは実装しない
- Related PeopleはPrimary Contactのみ
- Accountingは受注金額、請求金額、入金額のみ
- Timelineは主要イベントのみ
- Fileのバージョン管理は実装しない
- AI Summaryは手動実行のみ
- Project内からのMessage送信はConversation画面への遷移でもよい
- Project Completion Checkは簡易表示とする

---

## Future

正式版以降で以下を検討する。

- Project Template
- Recurring Project
- Team Member
- Role Management
- Project内チャット
- File Version Management
- 外部ストレージ連携
- Calendar View
- Gantt View
- Kanban View
- 稼働時間記録
- 収益分析
- 原価管理
- Project間の依存関係
- 自動バックアップ
- Project共有ページ
- Client Portal
- 承認フロー
- 電子契約
- 納品確認
- AIによる進行リスク候補
- AIによる過去Projectとの比較
- 類似ProjectからのTask提案

---

## Success Criteria

Project Detailを開いたユーザーが、画面を移動せずに以下を把握できること。

- この仕事は何か
- 誰との仕事か
- 最後に何を話したか
- 現在どこまで進んでいるか
- 次に何をするべきか
- 納期はいつか
- Taskは何が残っているか
- Documentは作成済みか
- 請求・入金は完了しているか
- 必要なFileはどこにあるか

Project Detailが、情報を管理する画面ではなく、実際に仕事を進めるWorkspaceとして機能することを目標とする。


# Tasks

## Layer

Work

---

## Purpose

Tasksは、複数のProjectにまたがる未完了Taskを一覧表示し、ユーザーが次に行う仕事を確認する画面である。

Project Detail内のTasks Sectionが、一つのProjectを進めるための作業一覧であるのに対し、Tasks画面はすべてのProjectを横断して行動を確認するための画面である。

Tasksは、細かな予定を大量に登録するTo Doアプリではない。

以下を明確にするためのAction Browserとして設計する。

```text
今、自分は何をするべきか
```

---

## Primary Goal

ユーザーがTasksを開いた時点で、今日または次に取り組むべき行動を迷わず選べること。

---

## Design Philosophy

Tasksでは、Taskの管理よりも行動の開始を優先する。

ユーザーに大量のTaskを並べて見せるのではなく、期限、Project、Conversationの状態を踏まえて、現在関係のあるTaskを見つけやすくする。

Taskは単独で存在することもできるが、可能な限り以下の文脈と関連付ける。

- Project
- Conversation
- Contact
- Document
- Accounting
- Calendar Event

Tasks画面はProject Detailの代替ではない。

Taskの背景や関連情報を詳しく確認する場合は、関連するProject DetailまたはConversationへ移動する。

---

## Task Definition

Taskは、ユーザーが実行または確認する必要がある一つの行動を表す。

Task名は、実行内容が理解できる動詞を含むことが望ましい。

良い例

```text
初稿PDFを山田先生へ送る
請求書の金額を確認する
8月公演の会場へ電話する
```

避ける例

```text
初稿
請求書
公演
```

Taskは、Project全体の状態や抽象的な目標を表すものではない。

---

## Task Types

Taskには以下の種類を設定できる。

- Action
- Reply
- Review
- Send
- Create
- Submit
- Follow Up
- Payment
- Waiting Check
- Other

Task Typeは、表示や候補生成を補助するための情報として使用する。

MVPではTask Typeを必須項目にしない。

---

## Task States

Taskは以下の基本ステータスを持つ。

- To Do
- In Progress
- Waiting
- Completed
- Cancelled

UIでは日本語表示を使用できる。

| Internal Status | UI |
|---|---|
| To Do | 未着手 |
| In Progress | 進行中 |
| Waiting | 待機中 |
| Completed | 完了 |
| Cancelled | 中止 |

---

## Waiting State

Waitingは、ユーザー自身の作業ではなく、外部の返答や状態変化を待っているTaskに使用する。

例

- 相手からの返信待ち
- 入金待ち
- 確認結果待ち
- 納品承認待ち
- 会場からの回答待ち

Waiting Taskには、必要に応じて確認予定日を設定できる。

確認予定日を過ぎても状態が変化していない場合、Follow Up候補を表示する。

---

## Screen Structure

Tasks画面の基本構造は以下とする。

```text
Tasks Header
↓
Quick Add
↓
Action Filters
↓
Task List
↓
Completed Tasks
```

Task Detailを独立した主要画面として必須にはしない。

Taskを選択した際は、Inline Detail、Bottom Sheet、Side Panelなどによって詳細を表示できる。

詳細なレイアウトは `11_Wireframes.md` で定義する。

---

## Tasks Header

画面上部に以下を表示する。

- 画面名
- Global Searchへの導線
- Filter
- Sort
- 表示設定
- Task作成

必要に応じて、現在表示しているTask数を表示できる。

---

## Quick Add

Taskを素早く作成する入力欄を表示する。

必須項目はTask名のみとする。

入力例

```text
＋ Taskを追加
```

Task名を入力して作成した後、必要に応じて以下を追加できる。

- Project
- Contact
- Conversation
- 期限
- ステータス
- Task Type
- メモ

Task作成前に詳細フォームへの入力を要求しない。

---

## Natural Language Input

Quick Addでは、自然文による入力を候補として解釈できる。

入力例

```text
明日までに山田先生へ初稿を送る
```

AIは以下を候補として抽出できる。

```text
Task名：初稿を山田先生へ送る
期限：明日
Contact：山田先生
```

抽出結果はユーザーが確認し、必要に応じて修正した後に保存する。

AIが解釈した内容を自動確定しない。

---

## Action Filters

Task Listの上部に、行動に基づくFilterを表示する。

基本Filter

- Today
- Upcoming
- Waiting
- Overdue
- Unscheduled
- All

UIでは日本語表示を使用できる。

| Filter | UI |
|---|---|
| Today | 今日 |
| Upcoming | 今後 |
| Waiting | 待機中 |
| Overdue | 期限超過 |
| Unscheduled | 期限なし |
| All | すべて |

---

## Today

今日対応するTaskを表示する。

対象

- 期限が今日のTask
- 期限を超過したTask
- 今日確認するWaiting Task
- ユーザーがTodayへ追加したTask
- 今日のNext Actionに設定されたTask

今日に多くのTaskが存在する場合も、AIが自動でTaskを延期または削除しない。

---

## Upcoming

将来の期限が設定されている未完了Taskを表示する。

日付ごとにグループ化できる。

表示例

```text
明日

□ 初稿PDFを送る
  ○○高校 編曲依頼

8月10日

□ 会場担当者へ確認する
  秋公演
```

---

## Waiting

Waiting状態のTaskを表示する。

表示内容

- Task名
- 待っている対象
- Waiting開始日
- 確認予定日
- 関連Project
- 関連Conversation

表示例

```text
○ 山田先生からの修正確認
○○高校 編曲依頼

3日前から待機中
明日確認予定
```

Waiting Taskから関連Conversationを直接開ける。

---

## Overdue

期限を過ぎた未完了Taskを表示する。

期限超過を過剰に責める表現は使用しない。

避ける表現

```text
失敗
重大な遅延
達成できていません
```

推奨する表現

```text
期限を過ぎています
日程を変更
完了にする
```

ユーザーは期限を変更せず、そのままTaskを継続できる。

---

## Unscheduled

期限が設定されていない未完了Taskを表示する。

Unscheduled Taskは不完全なTaskとして扱わない。

作業内容によっては期限を必要としないため、期限設定を強制しない。

---

## Task List

Taskをリスト形式で表示する。

各Taskの表示候補

- 完了Checkbox
- Task名
- 期限
- ステータス
- Project名
- Contact名
- Task Type
- 優先度
- Waiting情報
- 関連Conversation
- Subtask数
- AI候補表示

基本表示例

```text
□ 初稿PDFを山田先生へ送る
  ○○高校 編曲依頼
  今日
```

Waiting Taskの表示例

```text
○ 修正内容の返信を待つ
  ○○高校 編曲依頼
  3日前から待機中
```

---

## Task Grouping

Taskは以下の単位でグループ化できる。

- 日付
- Project
- ステータス
- Contact
- Task Type

初期状態ではAction Filterに応じて日付または状態でグループ化する。

Taskを細かい階層へ分けすぎない。

---

## Sorting

ユーザーは以下の条件で並び替えられる。

- Next Action
- 期限
- 作成日
- 最終更新
- Project
- 優先度
- 手動並び順

初期状態では、以下を考慮して表示する。

1. 期限超過
2. 今日が期限
3. Next Action
4. 確認予定日のWaiting Task
5. 将来の期限
6. 期限なし

AIによる並び順を利用する場合は、その旨を控えめに表示する。

---

## Filtering

ユーザーは以下で絞り込める。

- Project
- Contact
- ステータス
- Task Type
- 期限
- ラベル
- 優先度
- Conversationの有無
- Documentの有無
- 自分が作成したTask
- AI候補から作成したTask

MVPでは主要なFilterのみ実装する。

---

## Search

Tasks画面では以下の情報から検索できる。

- Task名
- Project名
- Contact名
- メモ
- 関連Conversation
- 関連Document
- ラベル

検索結果から関連するProject DetailまたはConversationへ移動できる。

---

## Task Detail

Taskを選択すると詳細を表示する。

表示内容

- Task名
- ステータス
- Task Type
- 期限
- 確認予定日
- Project
- Contact
- Conversation
- Document
- Accounting
- 親Task
- Subtask
- メモ
- 作成日時
- 完了日時

Task DetailはTaskの編集と関連情報へのアクセスを目的とする。

独立した複雑なWorkspaceにはしない。

---

## Task Editing

ユーザーは以下を編集できる。

- Task名
- ステータス
- 期限
- 確認予定日
- Project
- Contact
- Conversation
- Task Type
- 優先度
- メモ
- 親Task
- Subtask

変更は可能な限り自動保存する。

削除や大きな状態変更には取り消し手段を提供する。

---

## Task Completion

Checkboxまたは完了操作によってTaskをCompletedへ変更する。

完了後は短時間、取り消し操作を表示する。

表示例

```text
Taskを完了しました

元に戻す
```

Task完了時にProject全体を自動完了しない。

Task完了をConversationへ自動送信しない。

---

## Recurring Tasks

定期的に発生するTaskを設定できる。

例

- 毎月末に請求書を確認
- 毎週月曜に進捗を整理
- 入金予定日の翌日に確認

定期Taskでは、一つのTaskを再利用するのではなく、原則として各回のTask Instanceを生成する。

MVPではRecurring Tasksを実装対象外としてもよい。

---

## Subtasks

TaskにはSubtaskを設定できる。

例

```text
初稿を提出する

├ PDFを書き出す
├ ファイル名を確認する
├ メール文面を作る
└ 送信する
```

Subtaskを過剰に深い階層へ入れ子にしない。

MVPでは一階層のみとする。

---

## Next Action

TaskをProjectのNext Actionとして設定できる。

一つのProjectに設定できるNext Actionは、原則として一つとする。

新しいTaskをNext Actionに指定した場合、以前のTaskは通常の未完了Taskへ戻す。

Next Actionの設定は、Taskの優先度とは分けて管理する。

---

## Priority

Taskには任意で優先度を設定できる。

基本値

- High
- Normal
- Low

優先度を必須にしない。

優先度が未設定の場合も、期限やProjectの状態に基づいてTaskを表示できる。

すべてのTaskをHighにできないよう制限する必要はないが、Highの多用を促すUIにはしない。

---

## Task Relationships

Taskは以下の情報と関連付けられる。

### Project

Taskが属する仕事。

### Conversation

Taskが発生した会話、または対応に必要な会話。

### Contact

Taskの相手または関係者。

### Document

作成、確認、送信などの対象となる書類。

### Accounting

請求、支払い、入金確認などの対象。

### Calendar Event

Taskに関連する予定または締切。

一つのTaskにすべての関連情報を設定する必要はない。

---

## Conversation-Based Tasks

ConversationからTaskを作成できる。

例

```text
Message

「来週までに修正版をお願いします」
```

AIによる候補

```text
Task候補

修正版を提出する
期限：来週
```

ユーザーは以下を行える。

- Taskとして作成する
- 内容を編集する
- Projectへ紐づける
- 候補を無視する

AIがMessageを自動的にTaskへ変換しない。

---

## Document-Based Tasks

Documentの状態に基づいてTask候補を表示できる。

例

- 見積書を確認する
- 見積書を送付する
- 契約書へ署名する
- 請求書を作成する
- 請求書を送付する
- 入金を確認する

Documentの作成または状態変更によってTaskを自動確定しない。

---

## Accounting-Based Tasks

Accounting情報に基づいてTask候補を表示できる。

例

- 請求書を作成する
- 支払期限を確認する
- 入金を確認する
- 未入金について連絡する
- 経費を登録する

金銭に関するTaskは、関連するProject、Contact、Documentを明示する。

---

## Calendar Relationship

期限が設定されたTaskはCalendar上へ表示できる。

TaskとCalendar Eventは同一のデータとして扱わない。

- Taskは行動
- Calendar Eventは時間の予約

Taskに実行時間を確保する場合、TaskからCalendar Eventを作成できる。

Calendar Eventを削除しても、元のTaskを自動削除しない。

---

## Bulk Actions

複数のTaskを選択して以下を行える。

- 完了
- Project変更
- 期限変更
- ステータス変更
- ラベル追加
- 削除

MVPではBulk Actionsを実装対象外としてもよい。

---

## User Actions

ユーザーはTasks画面で以下を行える。

- Taskを作成する
- Taskを開く
- Taskを編集する
- Taskを完了する
- 完了を取り消す
- ステータスを変更する
- 期限を設定・変更する
- Waitingへ変更する
- 確認予定日を設定する
- Projectへ紐づける
- Conversationへ紐づける
- Contactへ紐づける
- Documentへ紐づける
- Next Actionに指定する
- Subtaskを追加する
- Taskを複製する
- Taskを削除する
- 検索する
- 絞り込む
- 並び替える

---

## AI Support

AIは以下を補助できる。

- ConversationからのTask候補
- Task名候補
- 期限候補
- Project候補
- Contact候補
- Task Type候補
- Waiting判定候補
- 確認予定日候補
- Next Action候補
- 重複Task候補
- Subtask候補
- Task分割候補
- 完了候補
- 日程変更候補

AIはTask候補を提示するが、自動作成しない。

AIはTaskの完了を推測できるが、自動完了しない。

AIはユーザーの許可なく期限を変更しない。

---

## Task Suggestions

AIによるTask候補は、独立した大きな受信箱として常時表示しない。

候補が発生した文脈の近くに表示する。

表示場所の例

- Conversation内
- Project Detail内
- Document Detail内
- Accounting内
- Tasks画面の小さな候補領域

Task候補には根拠を確認できる導線を設ける。

例

```text
Task候補

修正版を提出する
来週まで

元のMessageを確認
```

---

## AI Planning

AIは、複数のTaskを整理するための作業順候補を提示できる。

例

```text
今日の進め方候補

1. 初稿PDFを書き出す
2. 山田先生へ送信する
3. 請求書の金額を確認する
```

これは提案であり、自動的にTaskの優先度や期限を変更しない。

AI PlanningをTasks画面の中心には配置しない。

ユーザーが必要な場合に実行する補助機能とする。

---

## Navigation

Tasksから以下へ遷移できる。

- Project Detail
- Conversation
- Person Detail
- Document Detail
- Accounting
- Search
- Calendar

Taskの詳細を確認した後も、元のTask一覧の位置へ戻れること。

---

## Empty State

未完了Taskが存在しない場合は以下を表示する。

```text
現在対応するTaskはありません
```

新しいTaskを作成する導線を表示する。

Completed Taskが存在する場合は、完了履歴への導線を提供できる。

Filterによって結果がない場合は以下を表示する。

```text
この条件に該当するTaskはありません

Filterを解除
```

---

## Completed Tasks

完了済みTaskは未完了Taskと分けて表示する。

表示内容

- Task名
- 完了日時
- 関連Project
- 関連Contact

完了済みTaskは折りたたむ。

完了済みTaskを通常の一覧で強く目立たせない。

ユーザーは完了を取り消せる。

---

## Error State

Task取得に失敗した場合は以下を提供する。

- 再読み込み
- キャッシュ表示
- Task新規作成
- Filter解除

一部のProjectやConversation情報を取得できない場合も、Task名とローカルに保存された情報は表示する。

Task作成または更新に失敗した場合は、入力内容を失わないよう保持する。

---

## Loading State

Task Listのスケルトンを表示する。

キャッシュされたTaskが存在する場合は先に表示し、バックグラウンドで同期する。

Taskの完了やステータス変更では、画面全体を再読み込みしない。

---

## Offline Behavior

オフライン時も以下を可能にすることを検討する。

- Task一覧の閲覧
- Task作成
- Task編集
- Task完了

変更内容はローカルへ保存し、接続回復後に同期する。

競合が発生した場合は、ユーザーが内容を確認できるようにする。

MVPではOffline対応を限定的にしてもよい。

---

## Notifications

Taskの期限や確認予定日に基づいて通知できる。

通知候補

- 今日が期限
- 期限が近い
- 期限を過ぎた
- Waitingの確認予定日
- Next Action
- 定期Taskの作成

通知を過剰に送らない。

期限が設定されているだけで多数の通知を自動生成しない。

ユーザーが通知条件を変更できること。

---

## MVP Scope

MVPでは以下を実装する。

- Task一覧
- Today
- Upcoming
- Waiting
- Overdue
- Unscheduled
- Quick Add
- Task作成
- Task編集
- Task完了
- 完了取り消し
- ステータス変更
- 期限設定
- Projectとの紐付け
- Conversationとの紐付け
- Task Detail
- 検索
- 基本Filter
- 基本Sort
- Next Action指定
- ConversationからのTask候補
- AIによるTask名候補
- AIによる期限候補

---

## MVP Simplifications

MVPでは以下を簡略化できる。

- Task Typeは任意または未実装
- PriorityはNormal固定または未実装
- Subtaskは一階層のみ、またはFutureへ移動
- Recurring Tasksは実装しない
- Bulk Actionsは実装しない
- Calendar連携は実装しない
- Offline編集は実装しない
- Natural Language Inputは期限抽出のみでもよい
- AI Planningは実装しない
- Task DetailはBottom Sheetで表示する
- 通知は期限当日のみでもよい

---

## Future

正式版以降で以下を検討する。

- Recurring Tasks
- Subtask
- Bulk Actions
- Calendar連携
- Time Blocking
- 作業時間計測
- Task Template
- Project Templateとの連動
- チーム担当者
- Task共有
- Task依存関係
- Location Reminder
- 高度な自然言語入力
- 音声入力
- AIによる作業分解
- AIによる日程候補
- AIによる作業負荷候補
- AIによる類似Task検出
- 過去ProjectからのTask提案
- External Task Service連携
- Widget
- Wearable通知
- Offline同期
- カスタムTask Status

---

## Success Criteria

Tasksを開いたユーザーが、以下を迷わず把握できること。

- 今日何をするべきか
- 期限を過ぎているTaskはあるか
- 何を待っているか
- 次に確認するべき相手は誰か
- どのProjectに関係するTaskか
- Taskの背景となるConversationはどこか

Taskの確認から、完了、延期、返信、Project確認までが自然につながること。

Tasks画面が、仕事を細かく管理するための画面ではなく、ユーザーが次の行動を始めるための画面として機能することを目標とする。


# Documents

## Layer

Output

---

## Purpose

Documentsは、仕事に関連する見積書、請求書、契約書、納品書などを一覧表示する画面である。

ユーザーが書類を探し、作成し、送付状況や対応状況を確認するための入口として設計する。

Documentsは単なるファイル一覧ではない。

以下を把握するためのBusiness Document Browserである。

```text
どの書類を、
誰に、
何の仕事について、
どの状態で発行しているか
```

---

## Primary Goal

ユーザーが必要なDocumentを数秒で見つけ、作成、確認、送付、請求、入金確認などの次の行動へ進めること。

---

## Design Philosophy

Documentsでは、書類ファイルそのものだけでなく、書類が発生した仕事の文脈を重視する。

Documentは可能な限り、以下の情報と関連付ける。

- Project
- Contact
- Conversation
- Accounting
- Task

Documents画面は書類保管庫ではない。

書類を起点として、関連する仕事や金銭の状態へアクセスできるようにする。

---

## Document Definition

Documentは、仕事上の合意、依頼、納品、請求、支払いなどを記録または伝達する情報単位を表す。

Documentは必ずしもPDFファイルに限定しない。

Documentは以下を含む。

- 入力データ
- 表示内容
- 発行状態
- 送付状態
- 関連Project
- 関連Contact
- 関連Conversation
- PDFなどの出力ファイル
- 改訂履歴

ファイルはDocumentの出力形式または添付物として扱う。

---

## Document Types

基本的なDocument Typeは以下とする。

- Estimate
- Invoice
- Contract
- Delivery Note
- Purchase Order
- Receipt
- Statement
- Proposal
- Other

UIでは日本語表示を使用できる。

| Internal Type | UI |
|---|---|
| Estimate | 見積書 |
| Invoice | 請求書 |
| Contract | 契約書 |
| Delivery Note | 納品書 |
| Purchase Order | 発注書 |
| Receipt | 領収書 |
| Statement | 支払明細・取引明細 |
| Proposal | 提案書 |
| Other | その他 |

MVPでは、見積書と請求書を中心に実装する。

---

## Document States

Documentは以下の共通ステータスを持つ。

- Draft
- Ready
- Sent
- Viewed
- Accepted
- Rejected
- Paid
- Expired
- Cancelled
- Archived

UIではDocument Typeに応じて適切な名称を表示する。

| Internal Status | UI Example |
|---|---|
| Draft | 下書き |
| Ready | 発行準備済み |
| Sent | 送付済み |
| Viewed | 閲覧済み |
| Accepted | 承認済み |
| Rejected | 差し戻し |
| Paid | 入金済み |
| Expired | 期限切れ |
| Cancelled | 取消 |
| Archived | アーカイブ |

すべてのDocument Typeがすべてのステータスを使用する必要はない。

---

## Screen Structure

Documents画面の基本構造は以下とする。

```text
Documents Header
↓
Quick Status
↓
Document Type Filters
↓
Document List
↓
Archived Documents
```

---

## Documents Header

画面上部に以下を表示する。

- 画面名
- Search
- Filter
- Sort
- 表示設定
- Document作成

Document作成は画面内のPrimary Actionとして配置する。

---

## Quick Status

対応が必要なDocument数を小さく表示する。

表示候補

- 下書き
- 未送付
- 承認待ち
- 未請求
- 未入金
- 期限切れ

表示例

```text
下書き 2
未送付 1
未入金 3
```

各項目はDocument ListのFilterとして機能する。

Quick Statusを独立したダッシュボードにはしない。

---

## Document Type Filters

Document Typeごとに一覧を絞り込める。

基本Filter

- All
- Estimates
- Invoices
- Contracts
- Delivery Notes
- Other

UIでは日本語表示を使用する。

MVPでは以下のみでもよい。

- すべて
- 見積書
- 請求書
- その他

---

## Views

Documentsでは、目的に応じたViewを提供できる。

### Recent

最近作成または更新されたDocumentを表示する。

### Action Required

ユーザーの対応が必要なDocumentを表示する。

対象例

- Draft
- Readyだが未送付
- 差し戻し
- 期限切れ
- 未入金

### Sent

送付済みDocumentを表示する。

### Completed

承認、入金、処理が完了したDocumentを表示する。

### All

すべてのDocumentを表示する。

MVPではViewではなくFilterとして実装してもよい。

---

## Display

Document Listでは以下を表示する。

- Document名
- Document Type
- Document番号
- Contact
- Project
- 金額
- ステータス
- 発行日
- 送付日
- 支払期限または有効期限
- 最終更新日時

表示例

```text
請求書 #INV-2026-031

○○高校 編曲依頼
山田先生

100,000円
未入金
支払期限 8月31日
```

一覧ではすべての情報を表示せず、Document Typeと状態に応じて重要な情報を優先する。

---

## Document Naming

Documentにはユーザーが理解できる表示名を設定できる。

例

```text
○○高校 編曲料 請求書
U-Knot 10月公演 見積書
```

Document番号だけを主な表示名として使用しない。

Document番号は識別情報として補助表示する。

---

## Document Numbering

Document TypeごとにDocument番号を設定できる。

例

```text
EST-2026-001
INV-2026-001
CTR-2026-001
```

番号は自動採番できる。

ユーザーは必要に応じて番号を編集できる。

同一番号の重複を検出し、警告する。

自動採番の形式はSettingsから変更可能にすることを検討する。

---

## Sorting

ユーザーは以下の条件で並び替えられる。

- 最終更新
- 発行日
- 期限
- 金額
- Contact
- Project
- Document番号
- ステータス

初期状態では最終更新日時順とする。

Action Requiredでは、期限と対応状態を優先できる。

---

## Filtering

ユーザーは以下の条件で絞り込める。

- Document Type
- ステータス
- Project
- Contact
- 発行日
- 期限
- 金額
- 未送付
- 未請求
- 未入金
- ラベル
- 作成者
- アーカイブ状態

MVPでは主要項目のみ実装する。

---

## Search

Documents画面では以下から検索できる。

- Document名
- Document番号
- Contact名
- Project名
- 品目名
- 金額
- 備考
- メモ
- Conversation
- ラベル

検索結果からDocument Detail、Project Detail、Person Detailへ遷移できる。

---

## Document Creation

Documents画面から新しいDocumentを作成できる。

作成時は最初にDocument Typeを選択する。

例

```text
新しいDocument

見積書
請求書
契約書
納品書
その他
```

Document Typeの選択後、関連するProjectまたはContactを指定できる。

Projectから作成した場合は、Project情報を引き継ぐ。

---

## Quick Create

以下の情報だけでDocumentの下書きを作成できる。

- Document Type
- Contact
- Project
- 金額

その他の項目はDocument Detailで編集できる。

初回作成時にすべての項目を要求しない。

---

## Project-Based Creation

ProjectからDocumentを作成する場合、以下を候補として自動入力する。

- Project名
- Primary Contact
- 請求先Contact
- Project概要
- 受注金額
- 納期
- 品目
- Conversation
- 支払条件

ユーザーが内容を確認してから保存する。

---

## Conversation-Based Creation

Conversation内のMessageからDocument候補を作成できる。

例

```text
「今回の編曲料は10万円でお願いします」
```

AIによる候補

```text
見積書候補

宛先：山田先生
Project：○○高校 編曲依頼
金額：100,000円
```

AIはDocumentを自動発行または自動送付しない。

---

## Import

既存のDocumentファイルを取り込める。

対象例

- PDF
- 画像
- Word
- Excel

取り込み時は以下を設定できる。

- Document Type
- Contact
- Project
- 発行日
- 金額
- ステータス

AIはファイル内容から候補情報を抽出できる。

抽出結果はユーザーが確認する。

---

## User Actions

ユーザーはDocuments画面で以下を行える。

- Document Detailを開く
- Documentを作成する
- Documentを複製する
- Documentを削除する
- Documentをアーカイブする
- PDFを表示する
- PDFを書き出す
- Documentを送付する
- ステータスを変更する
- Projectへ紐づける
- Contactへ紐づける
- 検索する
- 絞り込む
- 並び替える
- 複数選択する

---

## Bulk Actions

複数のDocumentを選択して以下を行える。

- アーカイブ
- 削除
- ステータス変更
- Project変更
- Contact変更
- PDF出力
- ダウンロード

MVPではBulk Actionsを実装対象外としてもよい。

---

## AI Support

AIは以下を補助できる。

- ConversationからのDocument候補
- Document Type候補
- Contact候補
- Project候補
- 品目候補
- 金額候補
- 支払期限候補
- 有効期限候補
- 備考文候補
- メール文候補
- 入力漏れ候補
- 重複Document候補
- 請求漏れ候補
- ステータス変更候補
- インポートDocumentの情報抽出

AIはDocumentを自動発行、送付、承認、入金済みに変更しない。

---

## Navigation

Documentsから以下へ遷移できる。

- Document Detail
- Project Detail
- Person Detail
- Conversation
- Accounting
- Transaction Detail
- Search

---

## Empty State

Documentが存在しない場合は以下を表示する。

```text
まだDocumentがありません

Projectから見積書や請求書を作成するか、
新しいDocumentを作成してください。
```

Filterによって結果がない場合は以下を表示する。

```text
この条件に該当するDocumentはありません

Filterを解除
```

---

## Error State

Document一覧の取得に失敗した場合は以下を提供する。

- 再読み込み
- キャッシュ表示
- Document新規作成
- Filter解除

Document作成または編集に失敗した場合は、入力内容を失わないよう下書きとして保持する。

---

## Loading State

Document Listのスケルトンを表示する。

PDFサムネイルの読み込みを待たず、Document名や状態を先に表示する。

---

## MVP Scope

MVPでは以下を実装する。

- Document一覧
- 見積書
- 請求書
- その他Document
- Document名
- Document番号
- Contact
- Project
- 金額
- ステータス
- 発行日
- 支払期限
- Quick Status
- Document Type Filter
- 検索
- 基本Filter
- 基本Sort
- Document作成
- Document Detailへの遷移
- PDF表示
- PDF出力
- ProjectからのDocument作成
- AIによる入力候補

---

## MVP Simplifications

MVPでは以下を簡略化できる。

- Document Typeは見積書、請求書、その他のみ
- Viewは実装せずFilterで代用する
- Bulk Actionsは実装しない
- ImportはPDFのみ、またはFutureへ移動
- 自動採番形式は固定
- Viewedステータスは実装しない
- 複雑な承認フローは実装しない
- Document送付はメール添付またはファイル共有のみ
- PDFサムネイルは実装しない

---

## Future

正式版以降で以下を検討する。

- 契約書
- 納品書
- 発注書
- 領収書
- 支払明細
- 提案書
- Custom Document Type
- Document Template
- Template Marketplace
- 電子署名
- 電子契約
- Client Approval
- Client Portal
- Online Payment
- 閲覧履歴
- 送付履歴
- 開封通知
- 自動採番設定
- 複数通貨
- 多言語Document
- 税制設定
- インボイス制度対応
- 源泉徴収対応
- PDF以外の出力形式
- 外部会計サービス連携
- Document一括出力
- AIによるDocument比較
- AIによる契約リスク候補
- AIによる過去Document再利用

---

## Success Criteria

Documentsを開いたユーザーが、以下を迷わず把握できること。

- どのDocumentが存在するか
- 誰に対するDocumentか
- どのProjectに関係するか
- 送付済みか
- 承認済みか
- 請求済みか
- 入金済みか
- 次に対応するDocumentは何か

Documentの検索から、編集、送付、請求確認、Project確認までが自然につながること。

---

# Document Detail

## Layer

Output

---

## Purpose

Document Detailは、一つのDocumentを作成、編集、確認、発行、送付し、その後の状態を追跡する画面である。

Documentの内容だけでなく、Documentが発生したConversation、Project、Contact、Accountingの文脈を一つの画面にまとめる。

Document Detailは単なる帳票編集画面ではない。

書類に関する一連の仕事を完了するためのDocument Workspaceとして設計する。

---

## Primary Goal

ユーザーがDocument Detailを開くだけで、以下を把握し、必要な操作を実行できること。

- 何のDocumentか
- 誰に対するDocumentか
- どのProjectに関係するか
- 内容に不足や誤りがないか
- 現在どの状態か
- 送付済みか
- 次に何をするべきか
- Accountingへどう反映されるか

---

## Design Philosophy

Document Detailでは、入力フォームと完成形のDocumentを分離しすぎない。

ユーザーが入力した内容が、実際のDocument上でどのように表示されるかを確認しやすくする。

Documentの作成から送付までを一つの画面内で進められることを優先する。

基本構造は以下とする。

```text
Document Header
↓
Current Status
↓
Document Preview
↓
Primary Actions
↓
Document Content
↓
Recipient
↓
Project and Conversation
↓
Accounting
↓
Files and Attachments
↓
Notes
↓
History
↓
Document Settings
```

一つの長いスクロール画面を基本とする。

---

## Document Header

画面上部に以下を表示する。

- Document名
- Document Type
- Document番号
- ステータス
- Contact
- Project
- 最終更新日時

主な操作

- 編集
- Preview
- PDF出力
- 送付
- 複製
- ステータス変更
- メニュー

---

## Current Status

Documentの現在状態と次の行動を簡潔に表示する。

表示例

```text
現在の状態

下書き
未送付
入力不足 1件
```

または

```text
現在の状態

送付済み
支払期限 8月31日
未入金
```

表示候補

- ステータス
- 送付状態
- 承認状態
- 支払状態
- 有効期限
- 支払期限
- 入力不足
- 次のTask

---

## Document Preview

実際に発行されるDocumentのPreviewを表示する。

Previewでは以下を確認できる。

- レイアウト
- 宛名
- 発行者情報
- 品目
- 金額
- 税
- 日付
- 備考
- 支払情報
- ページ数

Previewと入力内容がリアルタイムまたは短い遅延で連動することが望ましい。

モバイルでは、縮小Previewまたは全画面Previewへの導線を表示する。

---

## Edit Mode

Document Detailでは、閲覧状態と編集状態を区別できる。

編集状態では以下を行える。

- テキスト入力
- 項目追加
- 項目削除
- 並び替え
- Contact変更
- Project変更
- 金額変更
- 税設定
- 支払期限設定
- 備考編集
- Template変更

変更は下書きとして自動保存する。

発行済みDocumentを編集する場合は、改訂版として扱うことを検討する。

---

## Primary Actions

Documentの状態に応じて、主要Actionを変更する。

### Draft

- 内容を編集
- Preview
- 発行準備完了

### Ready

- PDF出力
- 送付
- 下書きへ戻す

### Sent

- 送付履歴を確認
- 再送
- 承認済みにする
- 入金を記録
- 取消

### Accepted

- 請求書を作成
- 入金を記録
- 関連Taskを確認

### Paid

- 入金情報を確認
- 領収書を作成
- アーカイブ

Primary Actionは一つまたは二つに絞る。

多数のActionを常に同じ強さで表示しない。

---

## Document Content

Document Typeに応じた内容を編集する。

共通項目

- Document名
- Document番号
- 発行日
- 有効期限
- 支払期限
- 件名
- 品目
- 数量
- 単位
- 単価
- 税率
- 小計
- 税額
- 合計金額
- 備考
- 支払条件

Document Typeごとに不要な項目は表示しない。

---

## Line Items

見積書や請求書では複数の品目を登録できる。

各品目の項目

- 品目名
- 説明
- 数量
- 単位
- 単価
- 税率
- 金額

表示例

```text
編曲料
1式 × 100,000円
100,000円
```

ユーザーは品目を追加、削除、複製、並び替えできる。

---

## Amount Calculation

金額は入力内容に基づいて自動計算する。

計算対象

- 数量 × 単価
- 小計
- 割引
- 税
- 源泉徴収
- 調整額
- 合計金額

自動計算された金額をユーザーが確認できること。

計算結果と手動入力が矛盾する場合は警告する。

ユーザーの確認なしに金額を変更しない。

---

## Tax Settings

Documentごとに税設定を変更できる。

設定候補

- 課税
- 非課税
- 不課税
- 税率
- 内税
- 外税
- 品目ごとの税率
- 端数処理

MVPでは標準税率と外税計算を中心に実装する。

税務上の判断をAIが断定しない。

---

## Withholding Tax

源泉徴収が必要な取引では、源泉徴収額を設定できる。

表示候補

- 対象
- 税率
- 計算対象金額
- 源泉徴収額
- 差引請求額

源泉徴収の適用はユーザーが判断する。

AIは候補や注意事項を表示できるが、自動適用しない。

---

## Recipient

Documentの宛先情報を表示・編集する。

表示候補

- Contact名
- 法人名・団体名
- 部署
- 役職
- 担当者名
- 郵便番号
- 住所
- メールアドレス
- 敬称
- 請求先情報

Person DetailまたはProjectから情報を引き継げる。

Document上の宛先情報は、Contactの現在情報とは別にSnapshotとして保持する。

Contact情報が後から変更されても、発行済みDocumentの宛先を自動変更しない。

---

## Issuer

Documentの発行者情報を表示・編集する。

表示候補

- 氏名
- 屋号
- 法人名
- 郵便番号
- 住所
- 電話番号
- メールアドレス
- 登録番号
- 振込先
- 印影
- ロゴ

発行者情報はSettingsのProfileから引き継ぐ。

Documentごとに変更できることを検討する。

---

## Project and Conversation

Documentに関連するProjectとConversationを表示する。

表示内容

- Project名
- Primary Contact
- Projectステータス
- 最新Conversation
- Document作成の根拠となったMessage

ユーザーは以下を行える。

- Project Detailを開く
- Conversationを開く
- 関連Projectを変更する
- 関連Messageを確認する

---

## Accounting

Documentに関連するAccounting情報を表示する。

見積書の場合

- 見積金額
- 受注状態
- 受注金額
- 関連請求書

請求書の場合

- 請求金額
- 支払期限
- 入金済み金額
- 未入金金額
- 入金日
- 関連Transaction

Document Detailから入金を記録できる。

入金記録はAccounting側にも反映する。

---

## Document Relationships

Document同士を関連付けられる。

例

```text
見積書
↓
契約書
↓
請求書
↓
領収書
```

関連情報として以下を表示する。

- 元になったDocument
- 次に作成されたDocument
- 改訂前Document
- 改訂後Document
- 取消Document

見積書から請求書を作成する場合、品目や金額を引き継ぐ。

---

## Versioning

Documentを改訂した場合、Versionを記録できる。

表示例

```text
Version 3

Version 1　7月10日
Version 2　7月15日
Version 3　7月18日
```

Versionごとに以下を保持する。

- Document内容
- 発行日時
- 作成者
- PDF
- 変更内容
- 送付状態

MVPでは、発行前は同一下書きを更新し、発行後の編集時だけ複製または改訂版を作成する方式でもよい。

---

## Sending

Document DetailからDocumentを送付できる。

送付方法の候補

- Gmail
- その他のメール
- Conversation Source
- 共有リンク
- PDFダウンロード
- 外部共有

送付前に以下を確認する。

- 宛先
- 件名
- Message本文
- 添付ファイル
- Document Version
- 金額
- 期限

AIは送付Messageの下書きを作成できる。

ユーザーの確認なしに送付しない。

---

## Send Message

Document送付時のMessageを編集できる。

表示例

```text
山田様

お世話になっております。
○○高校編曲依頼の請求書をお送りします。

ご確認のほど、よろしくお願いいたします。
```

AIによる候補は下書きとして表示する。

送付後はConversationに送付記録を追加する。

---

## Send History

Documentの送付履歴を表示する。

表示内容

- 送付日時
- 送付先
- 送付方法
- Document Version
- Message
- 送信結果
- 開封状態

MVPでは送付日時、送付先、送信結果のみでもよい。

---

## Approval

見積書、契約書、提案書などでは承認状態を記録できる。

状態候補

- 未回答
- 承認
- 差し戻し
- 拒否
- 期限切れ

承認は以下の方法で記録できる。

- ユーザーによる手動記録
- Client Portal
- 共有リンク
- Conversationからの候補検出

AIは承認らしいMessageを検出できるが、自動承認しない。

---

## Payment Status

請求書では支払状態を表示する。

状態候補

- 未請求
- 請求済み
- 一部入金
- 入金済み
- 期限超過
- 取消

支払状態はDocument StatusとAccounting情報の両方から構成される。

ユーザーが入金を記録した時点で確定する。

---

## Attachments

Documentに関連する添付ファイルを追加できる。

例

- 明細
- 参考資料
- 契約条件
- 納品物
- 証明書
- 既存PDF

添付ファイルはDocument本体と区別する。

---

## Internal Notes

Documentに関する内部メモを保存できる。

例

- 金額確認中
- 次回から単価変更
- 支払条件は月末締め翌月末
- 郵送も必要
- 担当者確認済み

Internal Notesは相手に送付されない。

AIがConversationからNote候補を提示できる。

---

## History

Documentに関する主要イベントを時系列で表示する。

表示対象

- Document作成
- 編集
- ステータス変更
- PDF出力
- 送付
- 再送
- 承認
- 差し戻し
- 入金
- 取消
- アーカイブ
- Version作成

Historyから過去のVersionや送付内容を確認できる。

---

## Document Settings

Document固有の設定を表示する。

設定候補

- Template
- Language
- Currency
- Tax
- Numbering
- Date Format
- Rounding
- Signature
- Logo
- Payment Information
- Footer
- Access Permission

MVPでは基本的なTemplate、税、振込先のみでもよい。

---

## Validation

発行または送付前に入力内容を確認する。

確認対象

- 宛先
- 発行者
- Document番号
- 発行日
- 品目
- 金額
- 税
- 期限
- 振込先
- 関連Project

問題がある場合は警告を表示する。

警告が存在しても、法的またはシステム上不可能でない限り、ユーザーが発行を続行できるようにする。

---

## Duplicate Detection

類似するDocumentが存在する場合、重複候補を表示する。

判定候補

- Contact
- Project
- Document Type
- 金額
- 発行日
- 品目

AIは重複候補を提示するが、自動削除または統合しない。

---

## User Actions

ユーザーはDocument Detailで以下を行える。

- Documentを編集する
- Previewを確認する
- PDFを出力する
- Documentを送付する
- 送付Messageを編集する
- Documentを複製する
- Versionを作成する
- ステータスを変更する
- Projectへ紐づける
- Contactへ紐づける
- Conversationへ紐づける
- 添付ファイルを追加する
- Internal Noteを追加する
- 承認状態を記録する
- 入金を記録する
- 関連Documentを作成する
- Documentを取消する
- Documentをアーカイブする
- Documentを削除する

---

## AI Support

AIはDocument Detail内で以下を補助できる。

- Document Type候補
- Document名候補
- 件名候補
- Contact候補
- Project候補
- 品目候補
- 数量・単価候補
- 金額候補
- 税設定の注意候補
- 支払期限候補
- 有効期限候補
- 備考文候補
- 送付Message候補
- 入力不足候補
- 重複Document候補
- 承認Message候補
- 入金Message候補
- 関連Document候補
- Conversation要約
- インポート内容の抽出

AIは以下を行わない。

- 金額の自動確定
- 税区分の自動確定
- 源泉徴収の自動適用
- Documentの自動発行
- Documentの自動送付
- 承認状態の自動確定
- 入金状態の自動確定
- Documentの自動削除

---

## AI Content Generation

AIはDocument本文や品目説明の候補を生成できる。

例

```text
品目候補

吹奏楽編成への楽曲編曲
スコアおよびパート譜制作一式
```

生成内容はすべて編集可能な下書きとして扱う。

ユーザーが採用しない限りDocumentへ反映しない。

---

## Navigation

Document Detailから以下へ遷移できる。

- Documents
- Project Detail
- Person Detail
- Conversation
- Accounting
- Transaction Detail
- Tasks
- Search

---

## Empty State

新規Documentでは、作成の起点に応じた初期状態を表示する。

Projectから作成した場合

```text
Project情報をもとに下書きを作成しました
```

単独で作成した場合

```text
宛先またはProjectを選択してください
```

品目がない場合は以下を表示する。

```text
品目はまだありません

品目を追加
```

---

## Error State

Documentの取得に失敗した場合は以下を提供する。

- 再読み込み
- キャッシュされた下書き
- Documentsへ戻る
- ローカル保存内容の復元

PDF生成または送付に失敗した場合は以下を表示する。

- 失敗した処理
- 原因
- 再試行
- 別の送付方法
- 下書きの保持

送付に失敗しても、Document StatusをSentへ変更しない。

---

## Loading State

Document Headerとローカル保存された入力内容を優先して表示する。

Previewや関連情報は独立して読み込む。

PDF生成中は進行状態を表示し、編集内容を失わないようにする。

---

## Offline Behavior

オフライン時も下書きの作成と編集を可能にすることを検討する。

オンライン接続が必要な操作

- メール送付
- 共有リンク作成
- Client Portal更新
- 外部会計連携
- Cloud PDF保存

オフライン中の変更はローカルへ保存し、接続回復後に同期する。

MVPでは限定的な下書き保存のみでもよい。

---

## Permissions

チーム利用時はDocumentごとに権限を設定できる。

権限候補

- 閲覧
- 編集
- 発行
- 送付
- 入金記録
- 削除

MVPでは個人利用を前提とし、権限管理を実装しなくてもよい。

---

## MVP Scope

MVPでは以下を実装する。

- Document Header
- Current Status
- Document Preview
- Edit Mode
- 見積書
- 請求書
- その他Document
- Document番号
- 発行日
- Contact
- Project
- Line Items
- 数量
- 単価
- 税率
- 合計金額
- 支払期限
- 備考
- 発行者情報
- 振込先
- PDF出力
- メールまたは共有による送付
- Send Message編集
- 送付履歴
- Accounting Summary
- 入金記録への導線
- Internal Notes
- History
- AIによる入力候補
- AIによる送付Message候補
- 自動下書き保存

---

## MVP Simplifications

MVPでは以下を簡略化できる。

- Previewは固定Template一種類
- Versioningは発行後の複製で代用する
- Viewedステータスは実装しない
- Client Approvalは手動記録のみ
- Client Portalは実装しない
- 電子署名は実装しない
- 複数通貨は実装しない
- Languageは日本語固定
- 税率は標準税率と非課税のみ
- 源泉徴収は手動入力
- 端数処理は固定
- 送付方法はGmailまたはPDF共有のみ
- Permissionsは実装しない
- Offline対応はローカル下書き保存のみ
- Document Relationshipsは見積書から請求書への変換のみ

---

## Future

正式版以降で以下を検討する。

- 複数Template
- Custom Template Editor
- ロゴ・印影配置
- 電子署名
- 電子契約
- Client Approval
- Client Portal
- Online Payment
- Credit Card Payment
- 開封通知
- 閲覧履歴
- Version Comparison
- Multiple Currency
- Multiple Language
- 品目ライブラリ
- 価格表
- 割引設定
- 分割請求
- 定期請求
- 一部入金
- 返金
- Credit Note
- 外部会計サービス連携
- 税理士共有
- インボイス制度の高度対応
- 源泉徴収の自動計算候補
- 海外取引対応
- Access Permission
- Team Approval Flow
- AIによる契約書解析
- AIによる条件差分比較
- AIによる過去Document再利用
- AIによる未請求検出
- AIによる送付タイミング候補

---

## Success Criteria

Document Detailを開いたユーザーが、以下を一つの画面で把握し、操作できること。

- 何のDocumentか
- 誰に対するDocumentか
- どのProjectに関係するか
- 内容は正しいか
- 金額はいくらか
- 現在どの状態か
- 送付済みか
- 承認済みか
- 入金済みか
- 次に何をするべきか

Documentの作成から確認、PDF出力、送付、承認、入金確認までが自然につながること。

Document Detailが単なる書類編集画面ではなく、書類に関する仕事を完了するWorkspaceとして機能することを目標とする。


# Accounting

## Layer

Finance

---

## Purpose

Accountingは、仕事に関する収入、支出、請求、入金、支払いを一覧表示する画面である。

ユーザーが以下を把握するためのFinance Browserとして設計する。

```text
いくら請求して、
いくら入金されて、
いくら支払い、
何がまだ完了していないか
```

Accountingは、本格的な会計ソフトそのものではない。

Conversation、Project、Documentから生まれた金銭情報を整理し、請求漏れや入金確認など、仕事を完了するために必要な行動へつなげる。

---

## Primary Goal

ユーザーがAccountingを開いた時点で、以下を数秒で把握できること。

- 未請求の仕事があるか
- 未入金の請求があるか
- 期限を過ぎた入金があるか
- 支払い予定があるか
- 最近どのような入出金があったか
- 各金額がどのProjectやDocumentに関係するか

---

## Design Philosophy

Accountingは、勘定科目や仕訳を最初に見せる画面ではない。

ユーザーが実際に行う行動を中心に設計する。

```text
請求する
↓
入金を確認する
↓
支出を記録する
↓
取引をProjectへ紐づける
```

会計処理の詳細は必要に応じて表示し、通常利用では仕事の文脈を優先する。

Accounting上の金額は可能な限り以下と関連付ける。

- Project
- Contact
- Document
- Conversation
- Task

---

## Accounting Scope

Accountingでは、主に以下を扱う。

- 売上予定
- 請求
- 売掛金
- 入金
- 支出
- 買掛金・未払金
- 支払い
- 返金
- 経費
- Transaction
- Project別収支

確定申告や正式な帳簿作成に必要な高度な会計機能は、将来的な拡張または外部会計サービスとの連携で対応する。

---

## Core Concepts

Accountingでは、以下の概念を区別する。

### Expected Revenue

Project上で合意または予定されている売上金額。

まだ請求書が作成されていない場合を含む。

### Invoice Amount

請求書として発行された金額。

### Received Amount

実際に入金された金額。

### Expected Expense

Projectで発生予定の支出。

### Expense Amount

実際に支出として記録された金額。

### Transaction

実際に発生した入金または支払いの記録。

---

## Screen Structure

Accounting画面の基本構造は以下とする。

```text
Accounting Header
↓
Action Summary
↓
Period Summary
↓
Finance Filters
↓
Receivables
↓
Payables
↓
Transactions
↓
Project Summary
```

一つの長いスクロール画面を基本とする。

各Sectionは折りたたみ可能にできる。

---

## Accounting Header

画面上部に以下を表示する。

- 画面名
- 対象期間
- Search
- Filter
- Sort
- Transaction作成
- Expense作成

対象期間は以下から選択できる。

- 今月
- 先月
- 今年
- 前年
- 任意期間

---

## Action Summary

現在対応が必要な金銭情報を小さく表示する。

表示候補

- 未請求
- 未入金
- 入金期限超過
- 支払予定
- 未分類Transaction
- Project未紐付け
- Document未紐付け

表示例

```text
未請求 3
未入金 2
期限超過 1
未分類 4
```

各項目はAccounting一覧のFilterとして機能する。

Action Summaryを独立したダッシュボードにはしない。

---

## Period Summary

選択期間の金額を要約表示する。

表示候補

- 売上予定
- 請求済み
- 入金済み
- 未入金
- 支出
- 収支
- 未請求金額

表示例

```text
今月

請求済み　300,000円
入金済み　200,000円
未入金　　100,000円
支出　　　 40,000円
```

収支は以下を基本とする。

```text
入金済み金額 - 支出済み金額
```

売上予定や未入金を収支へ含める場合は、確定値と予定値を明確に分ける。

---

## Finance Views

Accountingでは以下のViewを提供する。

### Action Required

対応が必要な項目を表示する。

対象例

- 未請求
- 未入金
- 支払期限超過
- 未分類Transaction
- 未紐付けTransaction

### Receivables

請求および入金予定を表示する。

### Payables

支払い予定および支出を表示する。

### Transactions

実際に発生した入出金を表示する。

### Projects

Project単位の収支を表示する。

### All

すべての金銭情報を表示する。

MVPではViewをFilterとして実装してもよい。

---

## Receivables

受け取る予定の金額を表示する。

対象

- 未請求のProject
- 下書き請求書
- 送付済み請求書
- 未入金請求書
- 一部入金請求書
- 期限超過請求書

表示内容

- Contact
- Project
- Document
- 請求金額
- 入金済み金額
- 未入金金額
- 支払期限
- ステータス
- 最終Conversation日時

表示例

```text
○○高校 編曲依頼
山田先生

請求額　100,000円
未入金　100,000円
期限　8月31日
```

---

## Receivable States

Receivableは以下の状態を持つ。

- Unbilled
- Draft
- Invoiced
- Partially Paid
- Paid
- Overdue
- Cancelled

UIでは日本語表示を使用できる。

| Internal Status | UI |
|---|---|
| Unbilled | 未請求 |
| Draft | 請求書下書き |
| Invoiced | 請求済み |
| Partially Paid | 一部入金 |
| Paid | 入金済み |
| Overdue | 入金期限超過 |
| Cancelled | 取消 |

---

## Unbilled Work

Projectに金額が登録されているが、請求書が作成されていない場合、未請求候補として表示する。

表示内容

- Project名
- Contact
- 受注金額
- Projectステータス
- 納品状況
- 請求予定日
- 関連Conversation

AIは未請求候補を提示できる。

ユーザーが請求不要と判断した場合は、候補を非表示または対象外にできる。

AIが自動で請求書を作成・送付することはない。

---

## Payables

支払う必要がある金額を表示する。

対象例

- 外注費
- 会場費
- 交通費
- 出演料
- 機材費
- 制作費
- 未払い経費
- 返金予定

表示内容

- 支払先
- Project
- 金額
- 支払期限
- 支払状態
- 支払方法
- 関連Document
- メモ

表示例

```text
外注費
佐藤太郎

30,000円
8月20日支払予定
U-Knot 10月公演
```

---

## Payable States

Payableは以下の状態を持つ。

- Planned
- Unpaid
- Partially Paid
- Paid
- Overdue
- Cancelled

| Internal Status | UI |
|---|---|
| Planned | 支払予定 |
| Unpaid | 未払い |
| Partially Paid | 一部支払 |
| Paid | 支払済み |
| Overdue | 支払期限超過 |
| Cancelled | 取消 |

---

## Transactions

実際に発生した入金および支払いを一覧表示する。

Transactionは銀行口座、現金、カードなどで実際に発生した金銭移動を表す。

表示内容

- Transaction Type
- 金額
- 発生日
- Contact
- Project
- Document
- 支払方法または入金方法
- カテゴリ
- 確認状態
- メモ

表示例

```text
入金

100,000円
○○高校
8月25日

○○高校 編曲依頼
請求書 #INV-2026-031
```

---

## Transaction Types

Transaction Typeは以下とする。

- Income
- Expense
- Refund Received
- Refund Paid
- Transfer
- Adjustment

UIでは日本語表示を使用する。

| Internal Type | UI |
|---|---|
| Income | 入金 |
| Expense | 支出 |
| Refund Received | 返金受取 |
| Refund Paid | 返金支払 |
| Transfer | 口座振替 |
| Adjustment | 調整 |

MVPではIncomeとExpenseを中心に実装する。

---

## Transaction States

Transactionは以下の状態を持つ。

- Draft
- Pending
- Confirmed
- Reconciled
- Cancelled

| Internal Status | UI |
|---|---|
| Draft | 下書き |
| Pending | 確認待ち |
| Confirmed | 確認済み |
| Reconciled | 照合済み |
| Cancelled | 取消 |

銀行明細などから自動取得したTransactionは、初期状態をPendingとする。

ユーザーが確認した後にConfirmedまたはReconciledへ変更する。

---

## Transaction Creation

ユーザーは手動でTransactionを作成できる。

入力項目

- Transaction Type
- 金額
- 発生日
- Contact
- Project
- Document
- 支払方法
- カテゴリ
- メモ
- 添付ファイル

必須項目は以下とする。

- Transaction Type
- 金額
- 発生日

その他の情報は後から追加できる。

---

## Quick Expense

支出を素早く記録できる。

入力例

```text
＋ 支出を追加
```

最低限の入力

- 金額
- 日付
- 内容

追加候補

- Project
- Contact
- カテゴリ
- 支払方法
- 領収書
- メモ

初回入力時に複雑な会計項目を要求しない。

---

## Receipt Capture

領収書またはレシートの画像をTransactionへ添付できる。

AIは画像から以下を候補として抽出できる。

- 店舗名
- 日付
- 合計金額
- 税額
- 支払方法
- 品目
- カテゴリ

抽出結果はユーザーが確認した後に保存する。

AIが支出区分や税務上の扱いを自動確定しない。

---

## Transaction Matching

Transactionを以下と照合できる。

- Invoice
- Receivable
- Payable
- Project
- Contact
- Document

例

```text
銀行入金 100,000円
↓
請求書 #INV-2026-031
```

照合後は請求書の入金状態へ反映する。

一つのTransactionを複数のDocumentまたはProjectへ分割して紐づけることも将来的に検討する。

---

## Partial Payment

一つの請求書に対して複数回の入金を記録できる。

例

```text
請求金額　100,000円

1回目入金　60,000円
2回目入金　40,000円
```

入金状況

```text
入金済み　100,000円
未入金　　　　 0円
```

入金合計が請求金額を超える場合は警告する。

自動的に金額を修正しない。

---

## Combined Payment

一回の入金が複数の請求書に対応する場合、入金額を分割して紐づけられる。

例

```text
入金 150,000円

├ 請求書A 100,000円
└ 請求書B  50,000円
```

MVPでは一つのTransactionと一つのDocumentの紐付けのみでもよい。

---

## Project Summary

Project単位で金銭状況を表示する。

表示内容

- Project名
- Contact
- 受注金額
- 請求金額
- 入金金額
- 支出金額
- 未入金金額
- 収支
- ステータス

表示例

```text
○○高校 編曲依頼

受注　100,000円
入金　100,000円
支出　 10,000円
収支　 90,000円
```

Project SummaryからProject Detailへ遷移できる。

---

## Contact Summary

Contact単位の取引状況を確認できる。

表示候補

- 累計売上
- 期間内売上
- 未請求
- 未入金
- 支出
- 最終入金日
- 最終請求日

詳細はPerson Detailまたは絞り込み結果として表示する。

---

## Categories

Transactionにはカテゴリを設定できる。

基本カテゴリ例

### Income

- 売上
- 報酬
- 返金受取
- その他収入

### Expense

- 外注費
- 交通費
- 会場費
- 機材費
- 消耗品費
- 通信費
- 広告費
- 手数料
- 接待交際費
- その他経費

MVPでは簡易カテゴリを使用する。

正式な勘定科目との対応は将来的に設定可能とする。

---

## Payment Methods

Transactionには支払方法または入金方法を設定できる。

例

- 銀行振込
- 現金
- クレジットカード
- デビットカード
- 電子マネー
- QR決済
- その他

複数の銀行口座やカードはAccountとして管理することを将来的に検討する。

---

## Search

Accounting画面では以下から検索できる。

- Contact名
- Project名
- Document名
- Document番号
- Transaction内容
- 金額
- カテゴリ
- メモ
- 支払方法
- 入金方法

---

## Sorting

ユーザーは以下で並び替えられる。

- 発生日
- 支払期限
- 金額
- Contact
- Project
- ステータス
- 最終更新

初期状態では発生日または期限順とする。

Action Requiredでは期限超過を優先する。

---

## Filtering

ユーザーは以下で絞り込める。

- 期間
- Transaction Type
- ステータス
- Project
- Contact
- Document Type
- カテゴリ
- 支払方法
- 未請求
- 未入金
- 期限超過
- 未分類
- 未紐付け
- 金額範囲

---

## User Actions

ユーザーはAccounting画面で以下を行える。

- Transaction Detailを開く
- 入金を記録する
- 支出を記録する
- Transactionを編集する
- Transactionを取消する
- Projectへ紐づける
- Contactへ紐づける
- Documentへ紐づける
- Transactionを照合する
- 請求書を作成する
- 未請求を対象外にする
- 入金済みにする
- 一部入金を記録する
- 支払済みにする
- 領収書を追加する
- 検索する
- 絞り込む
- 並び替える
- データを書き出す

---

## AI Support

AIは以下を補助できる。

- 未請求Project候補
- 請求漏れ候補
- 未入金候補
- TransactionとInvoiceの照合候補
- TransactionとProjectの紐付け候補
- TransactionとContactの紐付け候補
- カテゴリ候補
- 支払方法候補
- 重複Transaction候補
- 領収書情報の抽出
- 不足情報候補
- 期限超過のFollow Up候補
- 入金確認Message候補
- 支出候補
- 異常金額候補

AIは以下を行わない。

- Transactionの自動確定
- 入金状態の自動確定
- 支払状態の自動確定
- カテゴリの自動確定
- 税区分の自動確定
- 請求書の自動発行
- 相手への自動連絡
- Transactionの自動削除

---

## Follow Up Support

支払期限を過ぎた請求について、AIは連絡文候補を作成できる。

表示例

```text
入金確認のMessage候補

山田様

お世話になっております。
先日お送りした請求書について、
現在のご確認状況をお伺いできますでしょうか。
```

候補はConversationの下書きとして作成する。

ユーザーの確認なしに送信しない。

---

## Data Import

以下のデータを取り込めることを将来的に検討する。

- 銀行明細
- クレジットカード明細
- CSV
- 会計ソフトデータ
- 電子マネー明細

取り込んだTransactionはPendingとして扱う。

AIまたはルールによる候補紐付けは行えるが、ユーザー確認前に確定しない。

---

## Data Export

Accounting情報を以下の形式で書き出せる。

- CSV
- Excel
- PDF
- 会計ソフト連携形式

MVPではCSV出力を中心とする。

---

## Navigation

Accountingから以下へ遷移できる。

- Transaction Detail
- Document Detail
- Project Detail
- Person Detail
- Conversation
- Tasks
- Search
- Settings

---

## Empty State

Accounting情報が存在しない場合は以下を表示する。

```text
まだ取引がありません

請求書から入金を記録するか、
新しい支出を追加してください。
```

Filterによって結果がない場合は以下を表示する。

```text
この条件に該当する取引はありません

Filterを解除
```

---

## Error State

Accounting情報の取得に失敗した場合は以下を提供する。

- 再読み込み
- キャッシュ表示
- Transaction新規作成
- Filter解除

Transactionの保存に失敗した場合は入力内容を保持する。

外部サービス連携に失敗した場合も、手動入力機能は利用可能とする。

---

## Loading State

Action Summaryとキャッシュ済み一覧を優先して表示する。

各Sectionは独立して読み込む。

Transactionの状態変更では画面全体を再読み込みしない。

---

## Offline Behavior

オフライン時も以下を可能にすることを検討する。

- Transaction一覧の閲覧
- Transaction作成
- 支出記録
- 領収書添付
- メモ編集

変更はローカル保存し、接続回復後に同期する。

銀行連携や外部会計サービス連携にはオンライン接続を必要とする。

---

## Privacy and Security

Accounting情報は機密性の高い情報として扱う。

以下を検討する。

- 生体認証
- 再認証
- 端末内暗号化
- 通信暗号化
- 書き出し時の確認
- 共有制限
- 操作履歴

MVPでは、アプリ全体の認証と安全な通信を最低要件とする。

---

## MVP Scope

MVPでは以下を実装する。

- Accounting Header
- 対象期間
- Action Summary
- Period Summary
- Receivables
- 未請求
- 未入金
- 入金期限超過
- Payables
- Transactions
- Income
- Expense
- Transaction作成
- Transaction編集
- Transaction Detailへの遷移
- Projectとの紐付け
- Contactとの紐付け
- Documentとの紐付け
- 請求書からの入金記録
- 一部入金
- Project Summary
- 検索
- 基本Filter
- 基本Sort
- CSV出力
- AIによる紐付け候補
- AIによる未請求候補
- AIによる領収書情報抽出

---

## MVP Simplifications

MVPでは以下を簡略化できる。

- 銀行口座連携は実装しない
- クレジットカード連携は実装しない
- 自動照合は候補表示のみ
- Transaction分割は実装しない
- Combined Paymentは実装しない
- Transferは実装しない
- RefundはExpenseまたはIncomeとして記録する
- 正式な複式簿記は扱わない
- 税区分は簡易入力とする
- Account管理は実装しない
- Payablesは支出予定と支払済みのみ
- Period Summaryは入金、支出、未入金のみでもよい
- Data ExportはCSVのみ
- Offline対応は下書き保存のみ

---

## Future

正式版以降で以下を検討する。

- 銀行口座連携
- クレジットカード連携
- 電子マネー連携
- 自動Transaction取得
- 自動照合候補
- Account管理
- 口座間振替
- Transaction分割
- Combined Payment
- 分割入金
- 定期支出
- 定期請求
- 返金
- 外貨
- 為替差損益
- 予算管理
- Cash Flow予測
- Project利益分析
- Contact別売上分析
- 月次レポート
- 年次レポート
- 確定申告サポート
- 青色申告対応
- 複式簿記
- 勘定科目
- 仕訳
- 税区分
- 消費税管理
- 源泉徴収管理
- 外部会計ソフト連携
- 税理士共有
- チーム権限
- 承認フロー
- AIによる異常取引候補
- AIによるCash Flow注意候補
- AIによる支出分類候補
- AIによる月次振り返り

---

## Success Criteria

Accountingを開いたユーザーが、以下を迷わず把握できること。

- まだ請求していない仕事は何か
- まだ入金されていない請求は何か
- 期限を過ぎた請求はあるか
- 最近どの入出金があったか
- 何に対して支出したか
- 各取引がどのProjectとContactに関係するか
- 次にどの金銭対応を行うべきか

Accounting画面が会計知識を要求する管理画面ではなく、仕事の金銭的な完了を支援する画面として機能することを目標とする。

---

# Transaction Detail

## Layer

Finance

---

## Purpose

Transaction Detailは、一つの入金または支出について、内容、関係者、Project、Document、確認状態を表示・編集する画面である。

Transactionを単独の金額として扱うのではなく、以下の文脈へ接続する。

- なぜ発生した金額か
- 誰との取引か
- どのProjectに関係するか
- どのDocumentに対応するか
- 確認済みか
- Accounting上どのように扱われるか

---

## Primary Goal

ユーザーがTransaction Detailを開くだけで、以下を把握し、必要な確認を完了できること。

- 入金か支出か
- 金額はいくらか
- いつ発生したか
- 誰との取引か
- どのProjectに関係するか
- どのDocumentと対応するか
- 確認済みか
- 不足情報があるか

---

## Design Philosophy

Transaction Detailでは、会計用語よりも取引の意味を優先する。

ユーザーが最初に確認する情報は以下である。

```text
このお金は何か
```

勘定科目や税区分などは必要に応じて後から表示する。

Transactionの確認、紐付け、編集を一つの画面で完了できることを重視する。

---

## Screen Structure

Transaction Detailの基本構造は以下とする。

```text
Transaction Header
↓
Current Status
↓
Amount and Date
↓
Match and Relationships
↓
Payment Information
↓
Category and Accounting
↓
Receipt and Attachments
↓
Internal Notes
↓
History
```

一つの長いスクロール画面を基本とする。

---

## Transaction Header

画面上部に以下を表示する。

- Transaction Type
- 金額
- ステータス
- 発生日
- Contact
- Project

主な操作

- 編集
- 確認済みにする
- Documentへ紐づける
- Projectへ紐づける
- 複製
- 取消
- メニュー

表示例

```text
入金

100,000円
8月25日

確認待ち
```

---

## Current Status

Transactionの確認状態と不足情報を表示する。

表示例

```text
現在の状態

確認待ち
請求書候補 1件
Project未確定
```

または

```text
現在の状態

照合済み
請求書 #INV-2026-031
```

表示候補

- Transaction Status
- 照合状態
- Project紐付け状態
- Contact紐付け状態
- Category状態
- 入力不足
- 重複候補

---

## Amount and Date

Transactionの基本情報を表示・編集する。

項目

- Transaction Type
- 金額
- 発生日
- 計上日
- Currency
- 内容
- Reference Number

MVPでは発生日と計上日を同一として扱ってもよい。

---

## Transaction Description

ユーザーが理解できる取引名を設定できる。

例

```text
○○高校 編曲料入金
東京公演 会場費
7月分 Adobe利用料
```

銀行明細の文字列のみを主な表示名として使用しない。

元の銀行明細情報がある場合は、Raw Dataとして保持する。

---

## Match and Relationships

Transactionに関連する情報を表示する。

関連候補

- Contact
- Project
- Document
- Receivable
- Payable
- Conversation
- Task

表示例

```text
関連情報

Project
○○高校 編曲依頼

Document
請求書 #INV-2026-031

Contact
山田先生
```

---

## Document Matching

TransactionをDocumentへ紐づけられる。

請求書との照合時には以下を比較する。

- 金額
- Contact
- 支払期限
- 発生日
- Project
- 振込名義
- Document番号

AIは照合候補と根拠を表示できる。

例

```text
照合候補

請求書 #INV-2026-031
一致度が高い候補

金額が一致
Contactが一致
支払期限の3日前
```

ユーザーが確認した後に照合する。

---

## Matching Result

TransactionとInvoiceを照合した場合、以下へ反映する。

- Invoiceの入金済み金額
- Invoiceの未入金金額
- Payment Status
- Projectの入金金額
- Accounting Summary

照合を解除した場合は、関連する状態も再計算する。

履歴は削除しない。

---

## Project Relationship

Transactionを一つまたは複数のProjectへ紐づけられる。

MVPでは一つのProjectのみでもよい。

Projectへ紐づけた場合、Project DetailのAccountingへ反映する。

Projectが不明な場合は未紐付けとして保持できる。

---

## Contact Relationship

Transactionの相手となるContactを設定できる。

Incomeの場合

- 顧客
- 依頼者
- 主催者
- 支払者

Expenseの場合

- 店舗
- 外注先
- 出演者
- 会場
- 取引先

Contactが未登録の場合は、新規Contact候補を作成できる。

自動作成しない。

---

## Payment Information

支払または入金に関する情報を表示する。

項目候補

- 支払方法
- 入金方法
- Account
- 振込名義
- Reference Number
- 手数料
- 決済日時
- 支払先
- 受取先

銀行明細から取得した場合は元データを保持する。

---

## Fees

Transactionに手数料が含まれる場合、別項目として記録できる。

例

```text
請求金額　100,000円
入金額　　 99,560円
振込手数料　　440円
```

差額の扱いをユーザーが選択できる。

候補

- 手数料として記録
- 値引きとして記録
- 未入金として残す
- その他の調整

AIが自動確定しない。

---

## Category and Accounting

Transactionの分類情報を表示・編集する。

項目候補

- Category
- Subcategory
- 勘定科目
- 税区分
- 事業利用割合
- 経費対象
- メモ

通常利用ではCategoryを優先表示する。

正式な会計項目は展開時または外部会計連携時に表示する。

---

## Business and Personal Use

個人事業主向けに、事業用と私用を区別できる。

候補

- 事業用
- 私用
- 一部事業用
- 未確認

一部事業用の場合は事業利用割合を設定できる。

例

```text
事業利用割合 50%
```

AIは候補を提示できるが、自動確定しない。

---

## Tax Information

必要に応じて税情報を設定できる。

項目候補

- 税込・税抜
- 税率
- 税額
- 課税区分
- インボイス対応
- 源泉徴収

税務上の判断をAIが断定しない。

MVPでは税額を任意入力としてもよい。

---

## Split Transaction

一つのTransactionを複数の用途へ分割できる。

例

```text
支出 15,000円

├ 交通費 5,000円
├ 会場費 8,000円
└ 手数料 2,000円
```

各分割項目に以下を設定できる。

- 金額
- Project
- Category
- Contact
- Tax
- メモ

MVPではSplit Transactionを実装対象外としてもよい。

---

## Receipt and Attachments

Transactionに証憑を添付できる。

対象

- 領収書
- レシート
- 振込明細
- 請求書
- 支払通知
- クレジットカード利用明細
- その他の証明資料

表示内容

- File名
- File Type
- 追加日時
- 抽出情報
- 関連Document

---

## Receipt Extraction

AIは領収書画像から以下を抽出候補として表示できる。

- 店舗名
- 日付
- 合計金額
- 税額
- 支払方法
- 品目
- 登録番号
- Category

抽出した値とTransaction情報が異なる場合は、差異を表示する。

ユーザーが採用する値を選択する。

---

## Internal Notes

Transactionに関する内部メモを追加できる。

例

- 振込名義が本人名義と異なる
- 2件分をまとめて入金
- 後日領収書を取得
- 私用分を含む
- Project確認中

Notesは相手に共有されない。

---

## Duplicate Detection

類似するTransactionが存在する場合、重複候補を表示する。

判定候補

- 金額
- 発生日
- Contact
- 支払方法
- Description
- Reference Number

AIは重複候補を提示するが、自動削除または統合しない。

---

## Confirmation

ユーザーはTransactionをConfirmedへ変更できる。

確認時に以下の不足情報を表示できる。

- Project未設定
- Contact未設定
- Document未設定
- Category未設定
- 証憑未添付
- 金額差異

不足情報があっても、ユーザーはTransactionを確認済みにできる。

システムが必要以上に確認を阻害しない。

---

## Reconciliation

TransactionとDocumentや銀行明細が対応していることを確認した場合、Reconciledへ変更できる。

Reconciledは以下を意味する。

```text
このTransactionが何の取引であるか確認できている
```

正式な銀行残高照合などの高度な意味は、将来的な会計機能で別途定義する。

---

## Cancellation

誤って作成したTransactionはCancelledへ変更できる。

取消時に関連する以下を更新する。

- Invoice Payment Status
- Receivable
- Payable
- Project Summary
- Accounting Summary

Transactionを完全削除する場合は、取り消しまたは確認操作を必要とする。

履歴を保持する。

---

## History

Transactionに関する主要イベントを表示する。

表示対象

- Transaction作成
- インポート
- 編集
- Project紐付け
- Contact紐付け
- Document照合
- 照合解除
- Category変更
- 確認
- 取消
- 添付ファイル追加

---

## User Actions

ユーザーはTransaction Detailで以下を行える。

- Transactionを編集する
- Transactionを確認済みにする
- Transactionを照合済みにする
- Contactへ紐づける
- Projectへ紐づける
- Documentへ紐づける
- Categoryを設定する
- 支払方法を設定する
- 証憑を追加する
- メモを追加する
- Transactionを分割する
- 手数料を記録する
- 重複候補を確認する
- 照合を解除する
- Transactionを複製する
- Transactionを取消する
- Transactionを削除する

---

## AI Support

AIはTransaction Detail内で以下を補助できる。

- Transaction Description候補
- Contact候補
- Project候補
- Document照合候補
- Receivable照合候補
- Payable照合候補
- Category候補
- 支払方法候補
- 税情報の注意候補
- 重複Transaction候補
- 領収書情報の抽出
- 不足情報候補
- 手数料候補
- Split候補
- 私用・事業用候補
- Conversation候補

AIは以下を行わない。

- Transactionの自動確認
- Documentとの自動照合
- Categoryの自動確定
- 税区分の自動確定
- 私用・事業用の自動確定
- Transactionの自動取消
- Transactionの自動削除

---

## AI Explanation

AIによる候補には、可能な範囲で根拠を表示する。

例

```text
請求書候補

#INV-2026-031

金額が一致
Contactが一致
支払期限に近い入金
```

ユーザーが候補を採用する前に、元情報を確認できること。

---

## Navigation

Transaction Detailから以下へ遷移できる。

- Accounting
- Document Detail
- Project Detail
- Person Detail
- Conversation
- Tasks
- Search

---

## Empty State

新規Transactionでは以下を表示する。

```text
取引内容を入力してください
```

Documentから作成した場合は、Document情報をもとに初期値を設定する。

例

```text
請求書情報をもとに入金記録を作成しました
```

証憑がない場合は任意の追加Actionを表示する。

```text
領収書・明細を追加
```

---

## Error State

Transaction取得に失敗した場合は以下を提供する。

- 再読み込み
- キャッシュされた情報
- Accountingへ戻る

保存に失敗した場合は入力内容を保持する。

Document照合に失敗した場合もTransaction自体を失わない。

---

## Loading State

Transaction Header、金額、日付を優先して表示する。

関連Project、Document、AI候補は独立して読み込む。

状態変更時に画面全体を再読み込みしない。

---

## Offline Behavior

オフライン時も以下を可能にすることを検討する。

- Transaction閲覧
- Transaction作成
- 編集
- Category設定
- 領収書添付
- メモ追加

Document照合や外部明細取得は、オンライン接続回復後に行う。

---

## Permissions

チーム利用時は以下の権限を設定できる。

- 閲覧
- 編集
- 確認
- 照合
- 取消
- 削除

MVPでは個人利用を前提とし、権限管理を実装しなくてもよい。

---

## MVP Scope

MVPでは以下を実装する。

- Transaction Header
- Current Status
- Income
- Expense
- 金額
- 発生日
- Description
- Contact
- Project
- Document
- Category
- 支払方法
- Internal Notes
- Receipt添付
- Transaction編集
- Transaction確認
- Document照合
- 照合解除
- Transaction取消
- History
- AIによるProject候補
- AIによるContact候補
- AIによるDocument照合候補
- AIによるCategory候補
- AIによる領収書情報抽出

---

## MVP Simplifications

MVPでは以下を簡略化できる。

- 一つのTransactionに一つのProject
- 一つのTransactionに一つのDocument
- Split Transactionは実装しない
- Combined Paymentは実装しない
- Currencyは日本円固定
- Account管理は実装しない
- 計上日は発生日と同一
- 勘定科目は実装しない
- 税区分は任意メモまたは簡易入力
- 事業利用割合は実装しない
- ReconciledはConfirmedと統合してもよい
- Permissionsは実装しない
- Offline対応は下書き保存のみ

---

## Future

正式版以降で以下を検討する。

- Split Transaction
- Combined Payment
- 複数Projectへの配分
- 複数Documentへの配分
- Account管理
- 銀行明細連携
- カード明細連携
- 自動照合候補
- 外貨
- 為替
- 返金
- 口座振替
- 手数料自動分離候補
- 勘定科目
- 税区分
- 事業利用割合
- 複式簿記
- 仕訳
- 証憑保存要件対応
- 電子帳簿保存対応
- チーム承認
- 税理士確認
- 操作権限
- AIによる異常取引候補
- AIによる重複検出
- AIによる証憑不足候補
- AIによる月次確認候補

---

## Success Criteria

Transaction Detailを開いたユーザーが、以下を迷わず理解できること。

- このTransactionは何か
- 入金か支出か
- 金額はいくらか
- いつ発生したか
- 誰との取引か
- どのProjectに関係するか
- どのDocumentと対応するか
- 確認済みか
- 次に何を確認するべきか

Transactionの記録から、Project、Document、Contactとの紐付け、確認、照合までが自然につながること。

Transaction Detailが単なる金額入力画面ではなく、仕事とお金の関係を確定する画面として機能することを目標とする。


# Search

## Layer

Utility

---

## Purpose

Searchは、Conversation、People、Projects、Tasks、Documents、Accountingなど、サービス内のすべての情報を横断して検索する画面である。

ユーザーが「どの画面に保存されているか」を意識せず、名前、言葉、金額、日付、書類番号などから必要な情報を見つけられるようにする。

Searchは、各画面の検索機能を置き換えるものではない。

以下のような状況で利用するGlobal Searchとして設計する。

```text
どこにあるか分からないが、
探したい情報は分かっている
```

---

## Primary Goal

ユーザーが検索対象の種類や保存場所を考えず、必要な情報へ数秒で到達できること。

---

## Design Philosophy

Searchでは、Database構造やデータモデルをユーザーへ意識させない。

ユーザーは自然な言葉で検索し、結果から仕事の文脈を理解できること。

検索結果は単なる文字列一致の一覧ではなく、以下の関係が分かる形で表示する。

- 誰との情報か
- どのProjectに関係するか
- どのConversationから発生したか
- どのDocumentやTransactionに接続するか

Search画面内で業務を完結させるのではなく、目的の画面へ移動するための入口として設計する。

---

## Search Scope

Global Searchでは以下を検索対象とする。

- Conversation
- Message
- Contact
- Project
- Task
- Document
- Transaction
- Accounting情報
- File
- Note
- Label

MVPでは以下を中心に実装する。

- Conversation
- Contact
- Project
- Task
- Document
- Transaction

---

## Searchable Fields

### Conversation and Message

- Conversation Group名
- Message本文
- 送信者
- Conversation Source
- 添付ファイル名
- ラベル
- 送受信日時

### Contact

- 名前
- 読み方
- 所属
- 部署
- 役職
- メールアドレス
- 電話番号
- Contact Channel
- メモ
- ラベル

### Project

- Project名
- 概要
- Contact
- Task
- Conversation
- Document
- メモ
- ラベル
- 金額

### Task

- Task名
- メモ
- Project
- Contact
- Conversation
- 期限
- ステータス

### Document

- Document名
- Document番号
- Document Type
- Contact
- Project
- 品目
- 金額
- 備考
- 発行日
- 支払期限

### Transaction

- Transaction名
- 金額
- Contact
- Project
- Document
- Category
- メモ
- 発生日
- 支払方法

---

## Screen Structure

Search画面の基本構造は以下とする。

```text
Search Header
↓
Search Input
↓
Recent Searches
↓
Search Suggestions
↓
Search Results
↓
Filters
```

検索開始後はSearch Resultsを中心に表示する。

---

## Search Header

画面上部に以下を表示する。

- 戻る
- 画面名
- Search Input
- Filter
- Search History削除

モバイルではSearch InputをHeaderの中心に配置できる。

---

## Search Input

検索語を入力する。

入力例

```text
山田先生
```

```text
10万円の請求書
```

```text
来週までに修正
```

```text
INV-2026-031
```

入力中に検索候補を表示できる。

---

## Natural Language Search

Searchは自然文による検索に対応できる。

例

```text
まだ入金されていない請求書
```

```text
山田先生と話した編曲の案件
```

```text
今月期限のTask
```

```text
10万円以上のProject
```

AIは自然文を検索条件へ変換する候補を提示する。

変換例

```text
検索条件

Document Type：請求書
Payment Status：未入金
```

AIによる解釈結果は表示し、ユーザーが修正できること。

---

## Recent Searches

過去の検索語を表示する。

表示内容

- 検索語
- 使用日時
- 適用したFilter

ユーザーは以下を行える。

- 再検索
- 個別削除
- 履歴をすべて削除

Search HistoryはSettingsから保存しない設定にできることを検討する。

---

## Search Suggestions

入力内容に応じて候補を表示する。

候補例

- Contact名
- Project名
- Document番号
- 最近開いた項目
- よく利用する検索
- ラベル
- Filter候補

入力例

```text
山田
```

候補

```text
山田太郎
山田先生とのConversation
○○高校 編曲依頼
```

AI候補と通常の文字列一致候補は、必要に応じて区別する。

---

## Search Results

検索結果を種類ごとに表示する。

基本構造

```text
Top Results
↓
Conversations
↓
People
↓
Projects
↓
Tasks
↓
Documents
↓
Transactions
```

結果が少ない場合は一つの統合リストとして表示できる。

結果が多い場合は種類ごとにSectionを分ける。

---

## Top Results

検索語に最も関連すると考えられる結果を表示する。

表示候補

- 完全一致
- 最近利用した結果
- 現在進行中のProject
- 要返信Conversation
- 対応が必要なDocument
- 期限が近いTask

AIによる関連度は検索結果の補助として使用する。

ユーザーが種類別一覧へ移動できること。

---

## Result Context

各検索結果には、該当した文字列だけでなく仕事の文脈を表示する。

Conversationの例

```text
山田先生

「来週までに修正版をお願いします」

○○高校 編曲依頼
昨日・LINE
```

Documentの例

```text
請求書 #INV-2026-031

○○高校 編曲依頼
山田先生

100,000円
未入金
```

Taskの例

```text
初稿PDFを山田先生へ送る

○○高校 編曲依頼
今日
```

---

## Highlighting

検索語と一致する部分を強調表示する。

長いMessageやNoteでは、一致部分の前後を抜粋して表示する。

機密情報を不必要に広く表示しない。

---

## Result Types

### Conversation Result

表示内容

- Conversation Group名
- 一致Messageの抜粋
- Contact
- Conversation Source
- 関連Project
- 送受信日時
- 要返信状態

選択するとConversationへ遷移し、該当Message付近を表示する。

### People Result

表示内容

- 名前
- 所属
- 役職
- Contact Channel
- 最新Conversation
- 進行中Project数

選択するとPerson Detailへ遷移する。

### Project Result

表示内容

- Project名
- Primary Contact
- ステータス
- 納期
- Next Action
- 金額

選択するとProject Detailへ遷移する。

### Task Result

表示内容

- Task名
- Project
- Contact
- 期限
- ステータス

選択するとTask Detailを開く。

### Document Result

表示内容

- Document名
- Document番号
- Type
- Contact
- Project
- 金額
- ステータス

選択するとDocument Detailへ遷移する。

### Transaction Result

表示内容

- Description
- Transaction Type
- 金額
- 発生日
- Contact
- Project
- Category

選択するとTransaction Detailへ遷移する。

---

## Filters

検索結果を以下で絞り込める。

- Result Type
- Date
- Contact
- Project
- Status
- Label
- Conversation Source
- Document Type
- Transaction Type
- Amount Range

Filterは検索対象に応じて動的に表示する。

例

Documentだけを表示している場合

- Document Type
- Status
- Issue Date
- Contact
- Project
- Amount

---

## Date Search

自然な日付表現で検索できる。

例

- 今日
- 昨日
- 今週
- 先月
- 7月
- 2026年8月
- 8月10日まで
- 過去30日

日付がMessage日時、Task期限、Document発行日など、どの項目へ適用されているかを表示する。

---

## Amount Search

金額を条件として検索できる。

例

```text
10万円
```

```text
5万円以上
```

```text
1万円から3万円
```

対象候補

- Project金額
- Document金額
- Transaction金額
- 未入金金額
- 支出金額

検索対象が曖昧な場合は種類別に結果を表示する。

---

## Search Within Context

特定の画面からSearchを開いた場合、検索範囲を引き継げる。

例

Project DetailからSearchを開いた場合

```text
○○高校 編曲依頼の中を検索
```

対象

- Conversation
- Task
- Document
- File
- Note
- Transaction

ユーザーはGlobal Searchへ切り替えられる。

---

## Saved Searches

よく使う検索条件を保存できる。

例

- 未入金の請求書
- 今週締切のTask
- 山田先生の進行中Project
- Project未紐付けTransaction

保存した検索はHomeのAction FiltersやShortcutとして利用できることを検討する。

MVPではSaved Searchesを実装対象外としてもよい。

---

## Search Actions

検索結果から対象画面へ遷移するほか、簡易Actionを提供できる。

例

Conversation

- 既読にする
- 返信を開く

Task

- 完了する
- 期限を変更する

Document

- PDFを表示する
- 送付する

Transaction

- 確認済みにする
- Projectへ紐づける

簡易Actionを増やしすぎず、基本は対象画面への遷移を優先する。

---

## User Actions

ユーザーはSearch画面で以下を行える。

- 検索語を入力する
- 自然文で検索する
- 候補を選択する
- Filterを設定する
- 並び替える
- 検索履歴を確認する
- 検索履歴を削除する
- 結果の種類を切り替える
- 結果を開く
- 保存検索を作成する
- Global SearchとContext Searchを切り替える

---

## Sorting

検索結果を以下で並び替えられる。

- 関連度
- 最終更新
- 日付
- 名前
- 金額
- 期限

初期状態では関連度を優先する。

種類別表示では、各種類に適した並び順を使用できる。

---

## AI Support

AIはSearch内で以下を補助できる。

- 自然文の検索条件への変換
- 検索語の補完
- 表記揺れの吸収
- Contact名候補
- Project候補
- Document Type候補
- Date Filter候補
- Amount Filter候補
- 類似表現検索
- 関連結果候補
- 検索結果の簡易要約
- 検索条件の修正候補

AIは検索結果そのものを書き換えない。

AIによる推測結果と実際に保存されている情報を区別する。

---

## Semantic Search

完全一致しない場合も、意味が近い情報を候補として表示できる。

例

検索語

```text
修正版を送る
```

候補

```text
Task
初稿を修正して再提出する
```

Semantic Searchを利用した結果には、必要に応じて一致理由を表示する。

MVPでは文字列検索を中心とし、Semantic Searchは段階的に実装できる。

---

## Search Result Summary

検索結果が多い場合、AIは結果を短く要約できる。

例

```text
山田先生に関連する情報が12件あります。

進行中Project 2件
要返信Conversation 1件
未入金Document 1件
```

Summaryは検索結果の代わりにしない。

元の結果へアクセスできること。

---

## Navigation

Searchから以下へ遷移できる。

- Conversation
- Person Detail
- Project Detail
- Task Detail
- Document Detail
- Transaction Detail
- Accounting
- Settings

遷移後、Searchへ戻った際に検索語、Filter、スクロール位置を維持する。

---

## Empty State

検索前は以下を表示する。

```text
何を探しますか？
```

補助例

```text
名前、Project、Message、Document番号、
金額などから検索できます。
```

最近の検索や最近開いた項目を表示できる。

---

## No Results State

検索結果がない場合は以下を表示する。

```text
一致する結果がありません
```

補助Action

- 検索語を変更
- Filterを解除
- Global Searchへ切り替え
- 表記を変えて検索
- 新しいContactまたはProjectを作成

AIは別の検索語候補を提示できる。

---

## Error State

検索に失敗した場合は以下を提供する。

- 再検索
- オフライン検索
- 最近開いた項目
- Filter解除

一部のデータソースの検索に失敗した場合も、取得できた結果は表示する。

---

## Loading State

入力中は短い遅延を設け、過剰な検索Requestを防ぐ。

検索結果のスケルトンを表示する。

追加結果の読み込みでは、既存結果を維持する。

---

## Offline Behavior

オフライン時は端末に保存されたデータを検索できることを検討する。

検索対象

- 最近のConversation
- Contact
- Project
- Task
- Document Metadata
- Transaction Metadata

オンライン限定の結果が含まれないことを表示する。

---

## Privacy

Search結果には機密情報が含まれる可能性がある。

以下を考慮する。

- ロック画面からの検索制限
- Search History保存設定
- 機密情報のPreview制限
- Shared Device Mode
- 生体認証
- 権限に応じた検索結果制御

---

## MVP Scope

MVPでは以下を実装する。

- Global Search
- Search Input
- Recent Searches
- Conversation検索
- Contact検索
- Project検索
- Task検索
- Document検索
- Transaction検索
- Result Type Filter
- Contact Filter
- Project Filter
- Date Filter
- Search Result Highlight
- Result Context表示
- 各Detail画面への遷移
- 検索状態の保持
- 基本文字列検索
- AIによる検索語候補
- AIによる自然文Filter候補

---

## MVP Simplifications

MVPでは以下を簡略化できる。

- Semantic Searchは限定的または未実装
- Saved Searchesは実装しない
- Search Result Summaryは実装しない
- Amount Searchは完全一致または範囲指定のみ
- Context SearchはProject Detail内のみ
- File本文検索は実装しない
- 添付ファイル内容検索は実装しない
- Search Actionsは対象画面への遷移のみ
- Offline Searchは最近のデータのみ
- Search Historyは端末単位で保存する

---

## Future

正式版以降で以下を検討する。

- 高度なSemantic Search
- Attachment全文検索
- PDF全文検索
- 音声Message検索
- 画像内文字検索
- File内容検索
- Saved Searches
- Smart Folder
- Search Shortcut
- Home Action Filtersとの連動
- Search Result Summary
- 複数条件の自然文検索
- 音声検索
- 表記揺れ辞書
- 略称辞書
- Custom Search Scope
- チーム横断検索
- Permission-aware Search
- Search Analytics
- AIによる検索意図推定
- AIによる関連Project探索
- AIによる過去案件比較

---

## Success Criteria

Searchを利用するユーザーが、以下を意識せず情報を見つけられること。

- どの画面に保存されているか
- どのData Modelに属するか
- どのConversation Sourceから来たか
- 正確な名称を覚えているか

検索結果から、対象のConversation、Person、Project、Task、Document、Transactionへ迷わず移動できること。

Searchが単なる文字列検索ではなく、仕事の文脈を横断して必要な情報へ到達する入口として機能することを目標とする。

---

# Settings

## Layer

Utility

---

## Purpose

Settingsは、ユーザー情報、外部サービス連携、通知、AI、Document、Accounting、Securityなど、サービス全体の動作を設定する画面である。

Settingsは、日常的に仕事を行うための画面ではない。

ユーザーがサービスを自分の業務環境へ適合させ、安心して利用するための管理画面として設計する。

---

## Primary Goal

ユーザーが設定項目の場所を迷わず見つけ、サービス全体の動作を安全に変更できること。

---

## Design Philosophy

Settingsでは、設定項目を機能単位ではなく、ユーザーが理解しやすい目的単位で分類する。

設定項目を一つの長い一覧へ無秩序に並べない。

基本カテゴリは以下とする。

```text
Account
Business Profile
Connections
Notifications
AI
Documents
Accounting
Data
Security
Appearance
Support
About
```

危険な操作や元に戻せない操作は、通常の設定から視覚的に分離する。

---

## Screen Structure

Settingsの基本構造は以下とする。

```text
Settings Header
↓
Account
↓
Business Profile
↓
Connections
↓
Notifications
↓
AI
↓
Documents
↓
Accounting
↓
Data and Storage
↓
Security and Privacy
↓
Appearance
↓
Support
↓
About
↓
Danger Zone
```

各カテゴリを選択すると、詳細画面または展開Sectionを表示する。

---

## Settings Header

画面上部に以下を表示する。

- 画面名
- Search Settings
- Account Summary
- 同期状態

Account Summary表示例

```text
川端 結
yu@example.com
```

---

## Settings Search

設定項目を検索できる。

検索対象例

- 通知
- Gmail
- 請求書
- 振込先
- AI
- パスワード
- データ出力
- ダークモード

検索結果から該当設定まで直接移動できる。

MVPでは設定カテゴリの文字列検索のみでもよい。

---

## Account

ユーザーアカウントに関する設定を行う。

項目候補

- 名前
- メールアドレス
- 電話番号
- Profile Image
- Login Method
- Password
- Language
- Time Zone
- Date Format
- Currency
- Logout

MVPでは日本語、日本時間、日本円を初期値として設定できる。

---

## Business Profile

仕事上の発行者情報を設定する。

項目候補

- 氏名
- 屋号
- 法人名
- 郵便番号
- 住所
- 電話番号
- メールアドレス
- Webサイト
- 登録番号
- ロゴ
- 印影
- 振込先
- 支払条件
- Document Footer
- Default Message

Business ProfileはDocument作成時の初期値として使用する。

既に発行済みのDocumentへ自動反映しない。

---

## Multiple Business Profiles

複数の屋号や活動名を使い分けられることを将来的に検討する。

例

- 個人名義
- U-Studio
- 匿名クリエイター名義

ProjectまたはDocumentごとにBusiness Profileを選択できる。

MVPでは一つのBusiness Profileのみとする。

---

## Connections

外部サービスとの接続を管理する。

接続候補

- Gmail
- Google Calendar
- Google Contacts
- Google Drive
- LINE
- Instagram
- Facebook
- X
- 銀行
- クレジットカード
- 会計ソフト
- Cloud Storage

各Connectionで以下を表示する。

- 接続状態
- 接続アカウント
- 最終同期日時
- 同期対象
- 権限
- 再接続
- 接続解除

---

## Connection Detail

Connectionごとに同期範囲を設定できる。

Gmailの例

- 同期するメールアドレス
- 対象期間
- 対象Label
- 除外する送信者
- 添付ファイル取得
- 送信権限
- 同期頻度

Google Calendarの例

- 同期するCalendar
- Task期限表示
- Project納期表示
- Calendar Event作成
- 双方向同期

MVPではConnectionごとの設定項目を限定できる。

---

## Connection Permissions

外部サービスへ付与している権限を確認できる。

例

- Message閲覧
- Message送信
- Contact閲覧
- Calendar閲覧
- Calendar作成
- File閲覧

ユーザーは接続解除または権限の再承認を行える。

権限が不足している場合は、その影響を具体的に表示する。

---

## Sync Status

各Connectionの同期状態を表示する。

状態候補

- 正常
- 同期中
- 一部エラー
- 認証切れ
- 停止中
- 未接続

表示内容

- 最終成功日時
- 最終試行日時
- 取得件数
- エラー内容
- 再試行

一つのConnectionが失敗しても、他の機能を停止しない。

---

## Notifications

通知設定を管理する。

通知カテゴリ

- Conversation
- Tasks
- Projects
- Documents
- Accounting
- Connections
- Security
- Product Updates

通知方法

- Push
- Email
- In-App
- Calendar

---

## Conversation Notifications

設定候補

- 新着Message
- 要返信候補
- 重要Conversation
- 特定Contact
- 特定Conversation Source
- 通知しない時間帯

AIによる要返信判定だけで過剰な通知を送らない。

---

## Task Notifications

設定候補

- 期限当日
- 期限前
- 期限超過
- Waiting確認予定日
- Next Action
- Recurring Task

ユーザーは通知タイミングを選択できる。

例

- 時刻指定
- 1日前
- 3日前
- 通知なし

---

## Document Notifications

設定候補

- 下書き未完了
- 未送付
- 有効期限
- 支払期限
- 承認
- 差し戻し
- 入金

---

## Accounting Notifications

設定候補

- 未請求
- 未入金
- 支払期限前
- 支払期限超過
- 支払予定
- 未分類Transaction
- 同期エラー

---

## Quiet Hours

通知しない時間帯を設定できる。

項目

- 開始時刻
- 終了時刻
- 曜日
- 緊急通知の扱い

MVPでは一つのQuiet Hoursのみでもよい。

---

## AI Settings

AI機能の動作を管理する。

基本原則は以下とする。

```text
AIは整理する
ユーザーが決める
```

AI Settingsで、AIが自動実行できるようにはしない。

設定できるのは主に提案範囲と表示方法である。

---

## AI Features

個別に有効・無効を設定できる。

- Conversation Summary
- Reply Suggestions
- Project Candidates
- Task Candidates
- Document Candidates
- Contact Candidates
- Duplicate Detection
- Search Assistance
- Accounting Matching
- Receipt Extraction
- Priority Suggestions

すべてのAI機能を一括で無効にできること。

---

## AI Suggestion Level

AI提案の表示量を設定できる。

候補

- Minimal
- Standard
- Proactive

UIでは以下のように説明する。

### Minimal

明示的に実行した場合のみAIを使用する。

### Standard

必要な場所に控えめな候補を表示する。

### Proactive

候補をより積極的に表示する。

どのLevelでも自動送信や自動確定は行わない。

---

## AI Data Usage

AI処理に使用される情報を説明する。

表示内容

- 使用するデータ
- 使用目的
- 外部AI Providerの有無
- 保存期間
- 学習利用の有無
- 無効化方法

ユーザーがAI処理の対象を制限できることを検討する。

例

- Message本文を使用しない
- AccountingをAI対象外にする
- 特定Conversationを除外する
- 添付ファイルを解析しない

---

## AI History

AIによる提案履歴を確認できることを将来的に検討する。

表示候補

- 提案内容
- 対象
- 採用・不採用
- 作成日時

MVPでは実装しなくてもよい。

---

## Document Settings

Document作成の初期設定を管理する。

項目候補

- Default Template
- Document Number Format
- Tax Rate
- Tax Calculation
- Rounding
- Currency
- Language
- Payment Terms
- Estimate Validity
- Invoice Due Date
- Default Notes
- Default Send Message
- Logo
- Signature
- Bank Information

---

## Document Number Settings

Document Typeごとに番号形式を設定できる。

例

```text
EST-{YYYY}-{000}
INV-{YYYY}-{000}
```

設定候補

- Prefix
- 年
- 月
- 連番
- 桁数
- リセット周期
- 次回番号

MVPでは固定形式を使用してもよい。

---

## Payment Terms

請求書の支払期限の初期値を設定する。

例

- 発行日から14日
- 発行日から30日
- 当月末
- 翌月末
- 手動設定

ProjectまたはContactごとに上書きできることを検討する。

---

## Accounting Settings

Accountingの基本設定を管理する。

項目候補

- 会計年度開始月
- Currency
- Default Category
- Payment Methods
- Accounts
- Tax
- Withholding Tax
- Business Use
- Export Format
- External Accounting Service

MVPでは以下を中心にする。

- Currency
- Category
- Payment Methods
- CSV Export

---

## Category Settings

収入・支出カテゴリを管理する。

ユーザーは以下を行える。

- Category追加
- 名前変更
- 並び替え
- 非表示
- Default設定

過去Transactionで使用中のCategoryを削除する場合は、別Categoryへの変更または非表示を推奨する。

---

## Payment Method Settings

支払方法を管理する。

例

- 銀行振込
- 現金
- クレジットカード
- 電子マネー
- QR決済

ユーザー独自の方法を追加できる。

---

## Data and Storage

データの保存、取込、書出、同期を管理する。

項目候補

- Storage Usage
- Cache
- File Storage
- Data Export
- Data Import
- Backup
- Restore
- Offline Data
- Sync Frequency

---

## Data Export

ユーザー自身のデータを書き出せる。

対象候補

- Contacts
- Projects
- Tasks
- Documents
- Transactions
- Conversations Metadata
- Files
- Settings

形式候補

- CSV
- JSON
- PDF
- ZIP

MVPでは主要データのCSV出力を提供する。

---

## Data Import

既存データを取り込める。

対象候補

- Contacts CSV
- Projects CSV
- Transactions CSV
- Documents
- Bank Statement
- Accounting Service Export

Import前にPreviewを表示し、重複候補を確認できること。

---

## Backup and Restore

データのBackupとRestoreを管理する。

候補

- Automatic Backup
- Manual Backup
- Backup History
- Restore Point

MVPではCloud上の通常保存のみで、ユーザー操作によるRestoreを実装しなくてもよい。

---

## Storage Usage

使用しているStorage容量を表示する。

内訳候補

- Message Attachments
- Documents
- Files
- Receipt Images
- Cache

容量削減Action

- Cache削除
- 古いAttachment削除
- Archive
- Export後削除

ユーザーの確認なしに元データを削除しない。

---

## Security and Privacy

アカウントと機密情報を保護する設定を管理する。

項目候補

- Password
- Passkey
- Two-Factor Authentication
- Biometric Lock
- App Lock
- Active Sessions
- Login History
- Connected Devices
- Privacy Settings
- Data Processing
- Search History
- AI Data Usage

---

## Biometric Lock

アプリ起動時または特定画面の表示時に生体認証を要求できる。

対象候補

- アプリ全体
- Accounting
- Documents
- Settings
- Data Export

MVPではアプリ全体のロックのみでもよい。

---

## Active Sessions

ログイン中の端末を表示する。

表示内容

- Device
- Location
- Last Active
- Login Method

ユーザーは他端末からログアウトできる。

---

## Privacy Settings

設定候補

- Search History保存
- Recent Items表示
- Notification Preview
- Attachment解析
- AI解析対象
- Analytics送信
- Crash Report
- Product Improvement Data

初期状態とデータ利用目的を明確に説明する。

---

## Appearance

表示に関する設定を管理する。

項目候補

- Theme
- Text Size
- Density
- Language
- Date Format
- Time Format
- Currency Display
- Accessibility

Theme候補

- System
- Light
- Dark

---

## Accessibility

アクセシビリティ設定を提供する。

項目候補

- 文字サイズ
- 高コントラスト
- 動きを減らす
- Screen Reader最適化
- 色以外による状態表示
- Touch Target拡大

重要なステータスを色だけで表現しない。

---

## Home Customization

Homeの表示を調整できることを将来的に検討する。

項目候補

- Action Filters
- Conversation Indicator
- FAB Menu
- Default Sort
- Hidden Labels

MVPではHome構造を固定してもよい。

---

## Labels

サービス全体で利用するLabelを管理する。

ユーザーは以下を行える。

- Label作成
- 名前変更
- 色変更
- 並び替え
- 非表示
- 削除

削除時は既存データからLabelが外れることを確認する。

AI MetadataはユーザーLabelとは別に管理する。

---

## Templates

再利用可能なTemplateを管理することを将来的に検討する。

対象候補

- Project
- Task
- Document
- Message
- Note
- Accounting Category

MVPではDocument Templateのみでもよい。

---

## Support

ユーザーが問題を解決するための情報を表示する。

項目

- Help Center
- FAQ
- Tutorial
- Contact Support
- Report a Problem
- Feature Request
- System Status

問い合わせ時に診断情報を添付する場合は、送信内容をユーザーへ表示する。

---

## Feedback

ユーザーは以下を送信できる。

- 不具合報告
- 改善要望
- AI提案へのFeedback
- Connection不具合
- Document出力不具合

個人情報やMessage本文を自動添付しない。

必要な場合はユーザーの確認を取る。

---

## About

サービス情報を表示する。

項目候補

- App Version
- Build Number
- Terms of Service
- Privacy Policy
- Licenses
- Copyright
- Open Source Licenses
- Update Information

---

## Subscription

料金プランを管理することを将来的に検討する。

項目候補

- Current Plan
- Usage
- Billing Cycle
- Payment Method
- Upgrade
- Downgrade
- Cancel Subscription
- Billing History
- Receipt

無料・有料機能の違いを具体的に表示する。

---

## Team Settings

チーム利用時の設定を将来的に提供する。

項目候補

- Organization
- Members
- Roles
- Permissions
- Invitations
- Shared Contacts
- Shared Projects
- Shared Documents
- Billing

MVPでは個人利用を前提とする。

---

## Danger Zone

元に戻すことが難しい操作を分離して表示する。

対象

- Connection解除
- 全データ削除
- Account削除
- Workspace削除
- AI History削除
- Search History削除
- Cache削除
- Reset Settings

Account削除では以下を行う。

- 再認証
- 削除対象の説明
- Exportへの導線
- 待機期間
- 最終確認

---

## Reset Settings

設定を初期状態へ戻せる。

対象を選択できることを検討する。

- Notification
- AI
- Appearance
- Document Defaults
- Accounting Defaults
- All Settings

ユーザーデータ自体は削除しない。

---

## User Actions

ユーザーはSettingsで以下を行える。

- Account情報を編集する
- Business Profileを編集する
- Connectionを追加・解除する
- 同期を再試行する
- 通知を設定する
- AI機能を有効・無効にする
- AI提案量を変更する
- Document初期値を設定する
- Accounting初期値を設定する
- Categoryを管理する
- Payment Methodを管理する
- DataをExportする
- DataをImportする
- Securityを設定する
- Appearanceを変更する
- Supportへ連絡する
- Logoutする
- Accountを削除する

---

## AI Support

Settingsでは、AIを設定項目の案内に利用できる。

例

```text
請求書の振込先を変更したい
```

AIは該当設定への導線を表示する。

AIが設定を自動変更する場合は、変更内容を明示してユーザー確認を取る。

AIは以下を行わない。

- Connectionの無断追加
- 権限の無断変更
- Data Export
- Data削除
- Account削除
- Security設定変更
- 有料プラン変更

---

## Settings Recommendation

サービス利用状況に応じて設定候補を表示できる。

例

```text
請求書を毎回30日後の期限で作成しています。
Default Payment Termsを30日に設定できます。
```

候補は控えめに表示し、自動変更しない。

---

## Navigation

Settingsから以下へ遷移できる。

- Connection Detail
- Business Profile
- Notification Settings
- AI Settings
- Document Settings
- Accounting Settings
- Security
- Data Export
- Data Import
- Support
- Subscription
- Home

---

## Empty State

Connectionがない場合は以下を表示する。

```text
まだ外部サービスが接続されていません
```

接続候補への導線を表示する。

Business Profileが未設定の場合は以下を表示する。

```text
発行者情報を設定すると、
見積書や請求書へ自動入力できます。
```

---

## Error State

設定の取得または保存に失敗した場合は以下を表示する。

- 保存できなかった項目
- 再試行
- 以前の設定
- Connection Status
- Supportへの導線

保存に失敗した場合、変更前の状態へ戻すか未保存状態を明示する。

---

## Loading State

Settingsの基本カテゴリを先に表示する。

Connection StatusやStorage Usageなどの外部情報は独立して読み込む。

一つのSectionの失敗で画面全体をブロックしない。

---

## Offline Behavior

オフライン時もローカル設定の変更を可能にする。

対象例

- Appearance
- Notification Preference
- AI表示設定
- Default Values

オンライン接続が必要な操作

- Connection追加
- Password変更
- Data Export
- Account削除
- Subscription変更

---

## Permissions and Confirmation

以下の操作は明示的な確認を必要とする。

- Connection解除
- Message送信権限の追加
- AI解析対象の拡大
- Data Export
- Data Import
- 全履歴削除
- Account削除
- Subscription変更

変更内容と影響を確認画面で説明する。

---

## MVP Scope

MVPでは以下を実装する。

- Settings Home
- Settings Search
- Account
- Business Profile
- Gmail Connection
- Google Calendar Connection
- Connection Status
- Notification Settings
- Conversation Notifications
- Task Notifications
- Document Notifications
- Accounting Notifications
- AI機能のOn・Off
- AI Suggestion Level
- Document Defaults
- 発行者情報
- 振込先
- Default Tax Rate
- Default Payment Terms
- Accounting Category
- Payment Methods
- CSV Export
- Search History削除
- Theme
- Logout
- Privacy Policy
- Terms of Service
- Account削除

---

## MVP Simplifications

MVPでは以下を簡略化できる。

- Business Profileは一つのみ
- ConnectionはGmailとGoogle Calendarを中心にする
- Connection Detailの設定項目は限定する
- NotificationはPushとIn-Appのみ
- Quiet Hoursは一つのみ
- AI Suggestion LevelはOn・Offのみでもよい
- Document Templateは一種類
- Document Number Formatは固定
- Accountingは簡易Categoryのみ
- Data Importは実装しない
- Data ExportはCSVのみ
- Backup Historyは実装しない
- Active Sessionsは実装しない
- Two-Factor AuthenticationはFutureへ移動可能
- Team Settingsは実装しない
- Subscriptionは外部Store管理でもよい
- Settings Recommendationは実装しない

---

## Future

正式版以降で以下を検討する。

- Multiple Business Profiles
- 複数Workspace
- LINE連携
- Instagram連携
- Facebook連携
- X連携
- 銀行連携
- クレジットカード連携
- 会計ソフト連携
- Cloud Storage連携
- Connectionごとの高度な同期条件
- Custom Notification Rules
- Custom AI Scope
- AI History
- AI Provider選択
- Local AI
- Multiple Document Templates
- Template Editor
- Custom Number Format
- Multiple Currency
- Multiple Language
- Account Management
- Backup and Restore
- Full Data Import
- Passkey
- Two-Factor Authentication
- Active Sessions
- Team Settings
- Role Management
- Permission Management
- Subscription Management
- Usage Limits
- Developer Settings
- API Access
- Webhook
- Automation Rules
- Settings Recommendation

---

## Success Criteria

Settingsを開いたユーザーが、以下を迷わず設定できること。

- 自分のアカウント
- 仕事上の発行者情報
- 外部サービス連携
- 通知
- AIの利用範囲
- Documentの初期設定
- Accountingの初期設定
- データのExport
- Security
- Privacy

設定変更の影響が分かり、危険な操作を誤って実行しないこと。

Settingsが複雑な管理画面ではなく、サービスを自分の仕事へ適合させるための安全な調整画面として機能することを目標とする。
