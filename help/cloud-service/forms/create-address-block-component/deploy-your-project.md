---
title: 住所コンポーネントの作成
description: AEM Forms as a Cloud Service での新しい住所コアコンポーネントの作成
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15752
exl-id: be25be52-2914-4820-9356-678a326f8edc
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 100%

---

# プロジェクトのデプロイ

AEM Forms as a Cloud Service へのプロジェクトのデプロイを開始する前に、ローカルのクラウド対応 AEM Forms インスタンスにプロジェクトをデプロイすることをお勧めします。

## 変更内容を AEM プロジェクトに同期する

以下に示すように、IntelliJ を起動し、``ui.apps`` フォルダーの下の adaptiveForm フォルダーに移動します
![intellij](assets/intellij.png)

``adaptiveForm`` ノードを右クリックして、新規 | パッケージを選択します
必ず名前 **addressblock** をパッケージに追加します

新しく作成されたパッケージ ``addressblock`` を右クリックし、以下に示すように ``repo | Get Command`` 選択します
![repo-sync](assets/sync-repo.png)

これにより、ロジェクトがローカルのクラウド対応 AEM Forms インスタンスと同期されます。.content.xml ファイルを検証して、プロパティを確認できます
![after-sync](assets/after-sync.png)

## ローカルインスタンスにプロジェクトをデプロイ

新しいコマンドプロンプトウィンドウを起動し、プロジェクトのルートフォルダーに移動して、以下に示すコマンドを使用してプロジェクトを作成します
![deploy](assets/build-project.png)

プロジェクトが正常にデプロイされると、
アダプティブフォームでアドレスコンポーネントを使用できるようになります

## 開発環境にプロジェクトをデプロイ

ローカル開発環境で問題がなければ、次の手順で Cloud Manager を使用して [クラウドインスタンスにデプロイします。](https://experienceleague.adobe.com/ja/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)
