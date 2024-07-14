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
exl-id: be25be52-2914-4820-9356-678a326f8edc
source-git-commit: a12b1778413079646814cb25567abfc26a429340
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# プロジェクトのデプロイ

AEM FormsCloud Serviceへのプロジェクトのデプロイを開始する前に、ローカルのAEM Forms cloud ready インスタンスにプロジェクトをデプロイすることをお勧めします。

## 変更内容のAEM プロジェクトとの同期

次に示すように、IntelliJ を起動し、``ui.apps`` フォルダーの下の adaptiveForm フォルダーに移動します。
![intellij](assets/intellij.png)

ノードを右クリック ``adaptiveForm`` て、「新規」を選択します。 | パッケージ
必ず名前 **addressblock** をパッケージに追加します

新しく作成されたパッケージ ``addressblock`` を右クリックし、次に示すよう ``repo | Get Command`` 選択します。
![repo-sync](assets/sync-repo.png)

これにより、プロジェクトがローカルクラウド対応AEM Forms インスタンスと同期されます。 .content.xml ファイルを確認して、プロパティを確認できます
![after-sync](assets/after-sync.png)

## ローカルインスタンスへのプロジェクトのデプロイ

新しいコマンドプロンプトウィンドウを起動し、プロジェクトのルートフォルダーに移動して、以下に示すコマンドを使用してプロジェクトを構築します
![deploy](assets/build-project.png)

プロジェクトが正常にデプロイされると、
アダプティブフォームで住所コンポーネントを使用できるようになりました

## クラウド環境へのプロジェクトのデプロイ

ローカル開発環境で問題がなければ、次の手順で cloud manager を使用して [cloud instance にデプロイします。](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)
