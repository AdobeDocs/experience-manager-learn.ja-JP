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
source-wordcount: '196'
ht-degree: 0%

---

# プロジェクトのデプロイ

AEM FormsCloud Serviceへのプロジェクトのデプロイを開始する前に、ローカルのAEM Forms cloud ready インスタンスにプロジェクトをデプロイすることをお勧めします。

## 変更内容のAEM プロジェクトとの同期

IntelliJ を起動し、の下の adaptiveForm フォルダーに移動します。 ``ui.apps`` フォルダー（下図を参照）
![intellij](assets/intellij.png)

右クリック ``adaptiveForm`` ノードして「新規」を選択します。 | パッケージ必ず名前を追加してください **addressblock** パッケージに追加

新しく作成されたパッケージを右クリックします。 ``addressblock`` を選択して、 ``repo | Get Command`` 下図のように
![リポジトリ同期](assets/sync-repo.png)

これにより、プロジェクトがローカルクラウド対応AEM Forms インスタンスと同期されます。 .content.xml ファイルを確認して、プロパティを確認できます
![after-sync](assets/after-sync.png)

## ローカルインスタンスへのプロジェクトのデプロイ

新しいコマンドプロンプトウィンドウを起動し、プロジェクトのルートフォルダーに移動して、以下に示すコマンドを使用してプロジェクトを構築します
![deploy](assets/build-project.png)

プロジェクトが正常にデプロイされると、住所コンポーネントをアダプティブフォームで使用できるようになります

## クラウド環境へのプロジェクトのデプロイ

ローカル開発環境で問題がなければ、次の手順はにデプロイします。 [cloud manager を使用したクラウドインスタンス。](https://experienceleague.adobe.com/en/docs/experience-manager-learn/cloud-service/forms/developing-for-cloud-service/push-project-to-cloud-manager-git)



