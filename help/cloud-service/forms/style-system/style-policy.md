---
title: AEM Forms でのスタイルシステムの使用
description: ボタンコンポーネントのスタイルポリシーの定義
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
exl-id: 52205a93-d03c-430c-a707-b351ab333939
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '139'
ht-degree: 100%

---

# コンポーネントのポリシーでスタイルを定義する

* ローカルクラウド対応 AEM インスタンスにログインし、ツール／一般／テンプレート／プロジェクト名の順に移動します。

* **コアコンポーネントを含む空白**&#x200B;テンプレートを選択し、編集モードで開きます。
* ボタンコンポーネントの「ポリシー」アイコンをクリックして、ポリシーエディターを開きます。

* ![button-policy](assets/button-policy.png)

次に示すように、ポリシーを定義します

![button-policy-details](assets/styling-policy.png)

Marketing と Corporate という 2 つのスタイル／バリエーションを定義しました。これらのバリエーションは、対応する CSS クラスに関連付けられています。**CSS クラス名の前後にスペースが入らないようにしてください**。
変更を保存します。

| スタイル | CSS クラス |
|-----------|------------------------------------|
| マーケティング | cmp-adaptiveform-button--marketing |
| Corporate | cmp-adaptiveform-button--corporate |

これらの CSS クラスは、コンポーネントの scss ファイル（_button.scss）で定義されます。

## 次の手順

[デフォルト CSS クラス](./create-variations.md)
