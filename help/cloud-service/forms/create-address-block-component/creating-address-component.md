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
exl-id: 280c9a30-e017-4bc0-9027-096aac82c22c
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: tm+mt
source-wordcount: '297'
ht-degree: 3%

---

# 住所コンポーネントの作成

ローカルのAEM Forms Cloud Ready インスタンスの CRXDE にログインします。

``/apps/bankingapplication/components/adaptiveForm/button`` ノードのコピーを作成し、名前を addressblock に変更します。 addressblock ノードを選択し、次に示すようにプロパティを設定します。

>[!NOTE]
>
> ``bankingapplication`` は、Maven プロジェクトの作成時に指定された appId です。 この appId は、お使いの環境で異なる可能性があります。 任意のコンポーネントのコピーを作成できます。たまたまボタンコンポーネントのコピーを作成しました


![address-bloc](assets/address-properties.png)

## cq-template ノードのプロパティ

``addressblock`` ノードの下の ``cq-template`` ノードを選択し、次に示すようにプロパティを設定します。 fieldType が panel に設定されています。
![cq-template](assets/cq-template.png)

## cq-template の下のノードの追加

``cq-template`` の下に、タイプ ``nt:unstructured`` の次のノードを追加します。

* streetaddress
* 市区町村
* zip
* ステート

これらのノードは、アドレスブロックコンポーネントのフィールドを表します。 streetaddress、city および zip フィールドは、テキスト入力フィールドになり、state フィールドは、ドロップダウンフィールドになります。

## streetaddress ノードのプロパティの設定

>[!NOTE]
>
> パスの **_bankingapplication_** は、Maven プロジェクトの appId を参照します。 これは、環境によって異なる場合があります

``streetaddress`` ノードを選択し、次に示すようにプロパティを設定します。
![ 番地 ](assets/streetaddress.png)

## 都市ノードのプロパティを設定します。

``city`` ノードを選択し、次に示すようにプロパティを設定します。
![ 市区町村 ](assets/city.png)

## zip ノードのプロパティの設定

``zip`` ノードを選択し、次に示すようにプロパティを設定します。
![zip](assets/zip.png)

## 状態ノードのプロパティの設定

``state`` ノードを選択し、次に示すようにプロパティを設定します。 状態の fieldType に注意してください。ドロップダウンに設定されています
![state](assets/state.png)

## 状態フィールドのデフォルト値を設定

``state`` ノードを選択して、次のプロパティを追加します。

| 名前 | タイプ | 値 |
|----------|----------|---------------------|
| 列挙 | 文字列[] | カリフォルニア、ニューヨーク |
| enumNames | 文字列[] | ニューヨーク州カリフォルニア |


最終的な addressblock コンポーネントは次のようになります

![ 最終アドレス ](assets/crx-address-block.png)

## 次の手順

[プロジェクトのデプロイ](./deploy-your-project.md)
