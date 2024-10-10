---
title: 住所コンポーネントの作成
description: AEM Forms Cloud Service での新しい住所コアコンポーネントの作成
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
exl-id: 280c9a30-e017-4bc0-9027-096aac82c22c
source-git-commit: 426020f59c7103829b7b7b74acb0ddb7159b39fa
workflow-type: ht
source-wordcount: '297'
ht-degree: 100%

---

# 住所コンポーネントの作成

AEM Forms のローカルクラウド対応インスタンスの CRXDE にログインします。

``/apps/bankingapplication/components/adaptiveForm/button`` ノードのコピーを作成し、名前を addressblock に変更します。addressblock ノードを選択し、次に示すようにプロパティを設定します。

>[!NOTE]
>
> ``bankingapplication`` は、Maven プロジェクトの作成時に提供された appId です。この appId は、環境によって異なる可能性があります。任意のコンポーネントのコピーを作成できます。例として、ボタンコンポーネントのコピーを作成しました。


![address-bloc](assets/address-properties.png)

## cq-template ノードのプロパティ

``addressblock`` ノードの下にある ``cq-template`` ノードを選択し、次に示すようにプロパティを設定します。fieldType が panel に設定されます。
![cq-template](assets/cq-template.png)

## cq-template の下へのノードの追加

``cq-template`` の下に ``nt:unstructured`` タイプの次のノードを追加します。

* streetaddress
* city
* zip
* state

これらのノードは、住所ブロックコンポーネントのフィールドを表します。streetaddress、city、zip のフィールドはテキスト入力フィールドになり、state フィールドはドロップダウンフィールドになります。

## streetaddress ノードのプロパティの設定

>[!NOTE]
>
> パス内の **_bankingapplication_** は、Maven プロジェクトの appId を参照します。これは、環境によって異なる可能性があります。

``streetaddress`` ノードを選択し、次に示すようにプロパティを設定します。
![street-address](assets/streetaddress.png)

## city ノードのプロパティの設定

``city`` ノードを選択し、次に示すようにプロパティを設定します。
![city](assets/city.png)

## zip ノードのプロパティの設定

``zip`` ノードを選択し、次に示すようにプロパティを設定します。
![zip](assets/zip.png)

## state ノードのプロパティの設定

``state`` ノードを選択し、次に示すようにプロパティを設定します。state の fieldType がドロップダウンに設定されます。
![state](assets/state.png)

## state フィールドのデフォルト値の設定

``state`` ノードを選択して、次のプロパティを追加します。

| 名前 | タイプ | 値 |
|----------|----------|---------------------|
| enum | 文字列[] | CA、NY |
| enumNames | 文字列[] | California、New York |


最終的な addressblock コンポーネントは次のようになります。

![final-address](assets/crx-address-block.png)

## 次の手順

[プロジェクトのデプロイ](./deploy-your-project.md)
