---
title: クラウドサービス設定とフォームデータモデルをクラウドインスタンスにプッシュしています
description: Azure ストレージフォームデータモデルに基づいてアダプティブフォームを作成し、クラウドインスタンスにプッシュします。
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: cloud-service
topic: Development
kt: 9006
source-git-commit: 8484897297940ab28619c4b1af5362a5937eadfa
workflow-type: tm+mt
source-wordcount: '202'
ht-degree: 0%

---


# プロジェクトにクラウドサービス設定を含める

クラウドサービス設定を保持する「FormsTutorial」という設定コンテナを作成します。「FormsTutorial」コンテナ内に、「Store Form Submissions in Azure」という Azure ストレージ用のクラウドサービス設定を作成します。 Azure ストレージアカウントの詳細とアカウントキーを入力します

IntelliJ でAEMプロジェクトを開きます。 ui.content プロジェクトで、以下に示すように、FormTutorial フォルダーを必ず追加してください。
![cloud-services-configuration](assets/cloud-services-configuration.png)

ui.content プロジェクトの filter.xml に次のエントリを必ず追加してください

```xml
<filter root="/conf/FormTutorial" mode="replace"/>
```

![filter-xml](assets/ui-content-filter.png)

## プロジェクトにフォームデータモデルを含める

前の手順で作成したクラウドサービス設定に基づいて、フォームデータモデルを作成します。 フォームデータモデルをプロジェクトに含めるには、AEMの intelliJ プロジェクトに適切なフォルダー構造を作成します。 例えば、フォームデータモデルが登録という名前のフォルダーにあるとします
![fdm-content](assets/ui-content-fdm.png)

ui.content プロジェクトの filter.xml に適切なエントリを含めます。

```xml
<filter root="/content/dam/formsanddocuments-fdm/registrations" mode="replace"/>
```


>[!NOTE]
>
>プロジェクトを構築してデプロイすると、クラウドインスタンスで使用可能なクラウドサービス設定に基づいたフォームデータモデルがプロジェクトに作成されます
