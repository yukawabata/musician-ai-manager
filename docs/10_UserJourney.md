# 10_UserJourney

# Purpose

本ドキュメントは、本サービスにおける代表的な仕事の流れ（User Journey）を定義する。

画面設計やUIは、このUser Journeyを自然に実現するために存在する。

新しい画面や機能を追加する際は、「このJourneyがよりスムーズになるか」を判断基準とする。

---

# Design Principle

本サービスは画面中心ではない。

仕事の流れを中心に設計する。

すべてのJourneyは以下の思想に従う。

```
会話
↓

案件

↓

作業

↓

書類

↓

会計

↓

完了
```

AIは各ステップで提案・整理・補助を行う。

ただし、意思決定は常にユーザーが行う。

---

# Journey 01

## 新しい仕事が来る

### Goal

依頼を受けてProjectを作成する。

### Flow

```
通知

↓

Home

↓

Conversation Group

↓

内容を確認

↓

AIが案件候補を提案

↓

ユーザー承認

↓

Project作成

↓

Task初期生成

↓

Project開始
```

### AI Support

AIは以下を提案できる。

- 案件候補
- クライアント候補
- 納期候補
- 金額候補
- タスク候補
- 優先順位

### User Action

ユーザーは

- Project名
- 納期
- 金額
- Contact

のみ確認すればよい。

---

# Journey 02

## 案件を進める

### Goal

案件を完了まで進める。

### Flow

```
Home

↓

Project

↓

Task確認

↓

Task完了

↓

必要に応じてDocument作成

↓

納品

↓

Project完了
```

### AI Support

AIは

- 今日やるべきTask
- 納期リスク
- 未返信
- 必要書類

を提案する。

---

# Journey 03

## 初めて仕事する相手

### Goal

Contactを自然に作成する。

### Flow

```
Conversation

↓

AIが新規Contact候補を提案

↓

プロフィール補完

↓

Contact作成

↓

Projectへ紐付け
```

### AI Support

AIは

- 名前
- メール
- 会社
- 部署
- 電話番号

などを抽出する。

---

# Journey 04

## 朝仕事を始める

### Goal

5分以内に仕事へ取り掛かれる。

### Flow

```
Home

↓

AI Summary

↓

今日やること

↓

未返信

↓

納期が近い案件

↓

重要案件

↓

仕事開始
```

### Homeに表示するもの

- 今日の優先事項
- 要返信
- AI提案
- 今日期限のTask
- 今週期限のProject
- 未回収金

ユーザーはHomeだけ見れば今日やることが分かる。

---

# Journey 05

## 請求書を発行する

### Goal

最短で請求書を発行する。

### Flow

```
Project

↓

Document

↓

Invoice

↓

AIが内容入力

↓

確認

↓

発行

↓

Accountingへ自動反映
```

### AI Support

AIは

- 宛先
- 明細
- 金額
- 消費税
- 支払期限

を提案する。

---

# Journey 06

## 入金を管理する

### Goal

未回収をなくす。

### Flow

```
Accounting

↓

未払い一覧

↓

入金確認

↓

ステータス更新

↓

完了
```

### AI Support

AIは

- 未払い
- 支払期限超過
- リマインド候補

を表示する。

---

# Journey 07

## 継続案件

### Goal

過去の仕事から次の案件へ繋げる。

### Flow

```
Home

↓

Conversation

↓

以前のProject表示

↓

新しい相談

↓

Project複製

↓

開始
```

### AI Support

AIは

- 過去案件
- 過去見積
- 前回金額
- 前回納期

を表示する。

---

# Journey 08

## 過去の仕事を探す

### Goal

必要な情報へ数秒で到達する。

### Flow

```
検索

↓

Contact

↓

Conversation

↓

Project

↓

Document

↓

Accounting
```

### Search対象

検索は以下を横断する。

- Contact
- Project
- Conversation
- Message
- Document
- Accounting
- Attachment

AI検索により自然言語検索も可能とする。

例

「去年の吹奏楽の請求書」

「○○高校とのやり取り」

「ラヴェル編曲」

---

# Journey 09

## AIだけを見る

### Goal

AI提案だけ確認して仕事を始める。

### Flow

```
Home

↓

AI Summary

↓

承認

↓

終了
```

### AI Summary例

- 案件候補
- 未返信
- Task候補
- 優先順位
- 今日やること
- 納期注意
- Document候補

Homeを5分見るだけで、その日の仕事を開始できることを目標とする。

---

# Journey 10

## 一日の仕事を終える

### Goal

今日の仕事を整理して終える。

### Flow

```
Home

↓

AI Daily Review

↓

今日完了した仕事

↓

未完了Task

↓

明日の予定

↓

終了
```

### AI Support

AIは

- 今日終わった仕事
- 明日やること
- 未返信
- 納期リスク

をまとめる。

---

# User Experience Goals

本サービスが実現したい体験は以下である。

## Before

```
LINE

↓

Gmail

↓

Notion

↓

Google Drive

↓

freee

↓

カレンダー

↓

Excel

↓

またLINE
```

仕事が分散している。

---

## After

```
Home

↓

Conversation

↓

Project

↓

Task

↓

Document

↓

Accounting

↓

完了
```

仕事が一つの流れになる。

---

# UX Principles

すべての画面設計は以下を満たすこと。

## Less Input

入力は最小限。

AIが候補を作る。

---

## Less Thinking

ユーザーに考えさせない。

今日やることはHomeが教える。

---

## Less Navigation

画面遷移を減らす。

必要な情報へ最短で到達できること。

---

## More Context

今見ている仕事に必要な情報を集約する。

Conversation

Project

Document

Accounting

を行き来しなくて済むUIを目指す。

---

## Human in Control

AIは提案する。

ユーザーが決める。

これが本サービス最大のUX思想である。

---

# Success Criteria

ユーザーは、

「どこに何があるか」

ではなく、

「今何をすればいいか」

だけを考えれば仕事が進む。

本サービスは、

仕事を管理するツールではない。

仕事が自然に流れる環境を提供するBusiness Relationship OSである。
