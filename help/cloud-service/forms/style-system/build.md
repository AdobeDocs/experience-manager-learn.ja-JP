---
title: AEM Forms でのスタイルシステムの使用
description: テーマプロジェクトの作成
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
workflow-type: ht
source-wordcount: '219'
ht-degree: 100%

---


# 変更内容のテスト

**「コアコンポーネントを含む空白」**テンプレートに基づいてアダプティブフォームを作成。フォームに 3 つのボタンをドラッグ＆ドロップし、「Corporate」、「Marketing」、「Default」というラベルを付けます。
図のようにペイントブラシを選択して、企業ボタンとマーケティングボタンに適切なスタイルバリアントを割り当てます

![styles](assets/marketing-variation.png)

## テーマプロジェクトの作成

次の手順では、テーマプロジェクトを作成します。テーマプロジェクトのルートフォルダーに移動し、次のスクリーンショットに示すように、コマンド「_**npm run build**_」を実行します

![build-theme](assets/build-theme.png)

テーマプロジェクトが正常に作成されたら、変更をテストする準備が整います。

## CSS を迅速かつ簡単にテストする方法

* テーマプロジェクトの dist フォルダーにある theme.css ファイルを開きます。ファイルの内容をすべて選択してコピーします。
* 前の手順で作成したフォームをプレビューします。
* いずれかのボタンを右クリックし、「Inspect」を選択して Developer Console を開きます。
* Developer Console で theme.css をクリックし、theme.css を開きます
* CTR-A を使用して theme.css のコンテンツ全体を選択して削除し、「削除」ボタンを押します。
* 前の手順で作成した theme.css のコンテンツをコピーして貼り付けます。
* ボタンは、次に示すように適切なスタイルで更新されます。

![final-buttons](assets/final-state-buttons.png)

