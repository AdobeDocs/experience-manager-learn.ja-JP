---
title: プロセスをトリガーするための初期フォームの作成
description: 署名プロセスを開始に送信する電子メール通知をトリガーするための初期フォームを作成します。
feature: アダプティブフォーム
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6892
thumbnail: 6892.jpg
topic: 開発
role: Business Practitioner
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 13%

---


# 初期フォームの作成

最初のフォーム（絞り込みフォーム）は、**複数のFormsに署名** AEMワークフローをトリガーして、複数のフォームに署名するために使用されます。 任意の値を入力できますが、次のフィールドがフォームに追加されていることを確認してください。



| フィールドタイプ | 名前 | 目的 | 非表示 | デフォルト値 |
------------------------|---------------------------------------|--------------------|--------|-----------------
| TextField | signed | 署名ステータスを示すには | ○ | × |
| TextField | guid | フォームを一意に識別するには | ○ | 3889 |
| TextField | customerName | 顧客名を取り込むには | × |
| TextField | customerEmail | 通知を送信する顧客の電子メール | × |
| CheckBox | formsToSign | アイテムはパッケージ内のフォームを識別します | × |



最初のフォームは、**signmultipleforms**と呼ばれるAEMワークフローをトリガーするように設定する必要があります
「データファイルのパス」が**Data.xml**&#x200B;に設定されていることを確認します。 これは、サンプルコードがフォームの送信を処理するペイロードでData.xmlというファイルを探すので、非常に重要です。

## Assets

最初のフォーム（絞り込みフォーム）は、[ここから](assets/refinance-form.zip)ダウンロードできます





