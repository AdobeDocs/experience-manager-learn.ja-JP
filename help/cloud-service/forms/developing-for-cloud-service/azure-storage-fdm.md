---
title: クラウドサービスの設定とフォームデータモデルをクラウドインスタンスにプッシュする
description: Azure ストレージフォームデータモデルに基づくアダプティブフォームを作成し、クラウドインスタンスにプッシュします。
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 9006
source-git-commit: d38da94bd4164163a16899b565c90b159194580a
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%

---


# プロジェクトにクラウドサービス設定を含める

クラウドサービス設定を格納する&#39;FormsTutorial&#39;という設定コンテナを作成します。&#39;FormsTutorial&#39;コンテナ内に、&#39;Store Form Submissions in Azure&#39;という Azure ストレージ用のクラウドサービス設定を作成します。 Azure ストレージアカウントの詳細とアカウントキーを入力します

IntelliJ でAEMプロジェクトを開きます。 次に示すように、ui.content プロジェクトに FormTutorial フォルダーを追加してください
![cloud-services-configuration](assets/cloud-services-configuration.png)

次のエントリを ui.content プロジェクトの filter.xml に追加してください

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## プロジェクトにフォームデータモデルを含める

前の手順で作成したクラウドサービス設定に基づいて、フォームデータモデルを作成します。 フォームデータモデルをプロジェクトに含めるには、IntelliJ のAEMプロジェクトに適切なフォルダ構造を作成します。 例えば、フォームデータモデルが登録と呼ばれるフォルダーにあるとします
![fdm-content](assets/ui-content-fdm.png)

ui.content プロジェクトの filter.xml に適切なエントリを含めます。

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]

>プロジェクトを構築してデプロイすると、クラウドインスタンスで使用可能なクラウドサービス設定に基づいたフォームデータモデルがプロジェクトに作成されます





