---
title: プロセスをトリガーする初期フォームの作成
description: メール通知をトリガーして署名プロセスを開始するための初期フォームを作成します。
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: User
level: Intermediate
jira: KT-6892
thumbnail: 6892.jpg
exl-id: d7c55dc8-d886-4629-bb50-d927308d12e3
duration: 35
source-git-commit: 8f3e8313804c8e1b8cc43aff4dc68fef7a57ff5c
workflow-type: tm+mt
source-wordcount: '177'
ht-degree: 100%

---

# 初期フォームの作成

最初のフォーム（借り換えフォーム）は、**複数のフォームに署名する** AEM ワークフローをトリガーして複数のフォームに署名するために使用されます。任意の値を入力できますが、以下のフィールドをフォームに必ず追加します。

| フィールドタイプ | 名前 | 目的 | 非表示 | デフォルト値 |
|--- |--- |---|--- |--- |
| TextField | signed | 署名ステータスを示す | ○ | × |
| TextField | guid | フォームを一意に識別する | ○ | 3889 |
| TextField | customerName | 顧客名を取得する | × | |
| TextField | customerEmail | 通知を送信する顧客のメール | × | |
| CheckBox | formsToSign | 項目でパッケージ内のフォームを識別する | × | |

初期フォームは、**signmultipleforms** と呼ばれる AEM ワークフローをトリガーするように設定する必要があります 。
データファイルのパスが **Data.xml** に設定されていることを確認します。サンプルコードではフォーム送信の処理時にペイロード内で Data.xml というファイルを探すので、これは非常に重要です。

## Assets

初期フォーム（借り換えフォーム）は、 [こちら](assets/refinance-form.zip)からダウンロードできます。

## 次の手順

[署名に使用するフォームの作成](./create-forms-for-signing.md)
