---
title: プロセスをトリガーする初期フォームの作成
description: 電子メール通知をトリガーする初期フォームを作成し、署名プロセスを開始します。
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
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 13%

---


# 初期フォームの作成

最初のフォーム（絞り込みフォーム）は、**複数のFormsに署名** AEMワークフローを呼び出して、複数のフォームに署名するために使用されます。 選択した値を入力できますが、次のフィールドがフォームに追加されていることを確認します。



| フィールドタイプ | 名前 | 目的 | 非表示 | デフォルト値 |
------------------------|---------------------------------------|--------------------|--------|-----------------
| TextField | signed | 署名ステータスを示すには | ○ | × |
| TextField | 素晴らしい | フォームを一意に識別するには | ○ | 3889 |
| TextField | customerName | 顧客名を取り込むには | × |
| TextField | customerEmail | 通知を送信する顧客の電子メール | × |
| CheckBox | formsToSign | 項目は、パッケージ内のフォームを識別します | × |



**signmultipleforms**と呼ばれるAEMワークフローをトリガーするには、最初のフォームを設定する必要があります
「データファイルのパス」が**Data.xml**&#x200B;に設定されていることを確認します。 これは、サンプルコードがフォーム送信処理のペイロード内でData.xmlというファイルを探すので、非常に重要です。

## Assets

最初のフォーム（絞り込みフォーム）は、[こちらからダウンロードできます。](assets/refinance-form.zip)





