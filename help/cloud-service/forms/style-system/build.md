---
title: AEM Formsでのスタイルシステムの使用
description: テーマプロジェクトの構築
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
source-git-commit: 86d282b426402c9ad6be84e9db92598d0dc54f85
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 2%

---


# 変更のテスト

「コアコンポーネントを使用した空白 **テンプレートに基づいてアダプティブフォーム** 作成します。 フォームに 3 つのボタンをドラッグ&amp;ドロップし、「Corporate」、「Marketing」、「Default」というラベルを付けます。
図のようにペイントブラシを選択して、適切なスタイルバリアントを企業ボタンとマーケティングボタンに割り当てます

![ スタイル ](assets/marketing-variation.png)

## テーマプロジェクトの構築

次の手順では、テーマプロジェクトを構築します。 テーマプロジェクトのルートフォルダーに移動し、次のスクリーンショットに示すように、コマンド _**npm run build**_ を実行します

![build-theme](assets/build-theme.png)

テーマプロジェクトが正常に作成されたら、変更をテストする準備が整います。

## CSS を迅速かつ簡単にテストする方法

* テーマプロジェクトの dist フォルダーにある theme.css ファイルを開きます。ファイルの内容をすべて選択してコピーします。
* 前の手順で作成したフォームをプレビューします。
* いずれかのボタンを右クリックし、「Inspect」を選択してデベロッパーコンソールを開きます。
* 開発者コンソールで theme.css をクリックし、theme.css を開きます。
* CTR-A を使用して theme.css のコンテンツ全体を選択して削除し、「削除」ボタンを押します。
* 前の手順で作成した theme.css のコンテンツをコピーして貼り付けます。
* ボタンは、次に示すような適切なスタイルで更新される必要があります。

![ 最終ボタン ](assets/final-state-buttons.png)

