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


