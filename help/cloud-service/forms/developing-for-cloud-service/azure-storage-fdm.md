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
exl-id: 77c00a35-43bf-485f-ac12-0fffb307dc16
source-git-commit: 2ac0f6b3964590e5443700f730a3fc02cb3f63bc
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 0%

---

# プロジェクトにクラウドサービス設定を含める

クラウドサービス設定を保持する「FormTutorial」という設定コンテナを作成する Azure ストレージのクラウドサービス設定を作成し、Azure ストレージアカウントの詳細と Azure アクセスキーを指定して、「FormsTutorial」コンテナ内に「FormsCSAndAzureBlob」という名前を付けます。

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
>Cloud Manager を使用してプロジェクトをビルドおよびデプロイする際に、クラウドサービス設定で Azure アクセスキーを再入力する必要があります。 アクセスキーを再入力しないように、環境変数を使用してコンテキスト対応設定を作成することをお勧めします。詳しくは、 [次の記事](./context-aware-fdm.md)
