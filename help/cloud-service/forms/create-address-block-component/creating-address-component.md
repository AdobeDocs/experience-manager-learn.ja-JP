---
title: 住所コンポーネントの作成
description: AEM Forms Cloud Serviceでの新しいアドレスコアコンポーネントの作成
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
source-git-commit: a8fc8fa19ae19e27b07fa81fc931eca51cb982a1
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 1%

---


# 住所コンポーネントの作成

ローカルのAEM Forms Cloud Ready インスタンスの CRXDE にログインします。

のコピーを作成 ``/apps/bankingapplication/components/adaptiveForm/button`` ノードを追加して、名前を addressblock に変更します。 addressblock ノードを選択し、次に示すようにプロパティを設定します。

>[!NOTE]
>
> ``bankingapplication`` は、Maven プロジェクトの作成時に指定された appId です。 この appId は、お使いの環境で異なる可能性があります。 任意のコンポーネントのコピーを作成できます。たまたまボタンコンポーネントのコピーを作成しました


![address-bloc](assets/address-properties.png)

## cq-template ノードのプロパティ

「」を選択します ``cq-template`` の下のノード ``addressblock`` をノードし、次に示すようにプロパティを設定します。 fieldType が panel に設定されています。
![cq-template](assets/cq-template.png)

## cq-template の下のノードの追加

タイプの次のノードを追加します ``nt:unstructured`` 未満 ``cq-template``

* streetaddress
* 市区町村
* zip
* ステート

これらのノードは、アドレスブロックコンポーネントのフィールドを表します。 streetaddress、city および zip フィールドは、テキスト入力フィールドになり、state フィールドは、ドロップダウンフィールドになります。

## streetaddress ノードのプロパティの設定

>[!NOTE]
>
> この **_bankingapplication_** のパスでは、Maven プロジェクトの appId を参照します。 これは、環境によって異なる場合があります

「」を選択します ``streetaddress`` をノードし、次に示すようにプロパティを設定します。
![番地](assets/streetaddress.png)

## 都市ノードのプロパティを設定します。

「」を選択します ``city`` をノードし、次に示すようにプロパティを設定します。
![市](assets/city.png)

## zip ノードのプロパティの設定

「」を選択します ``zip`` をノードし、次に示すようにプロパティを設定します。
![郵便番号](assets/zip.png)

## 状態ノードのプロパティの設定

「」を選択します ``state`` をノードし、次に示すようにプロパティを設定します。 状態の fieldType に注意してください。ドロップダウンに設定されています
![state](assets/state.png)

最終的な addressblock コンポーネントは次のようになります

![final-address](assets/crx-address-block.png)

## 次の手順

[プロジェクトのデプロイ](./deploy-your-project.md)




